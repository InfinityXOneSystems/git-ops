# GitHub Autonomous CI/CD Implementation Guide

## Overview

This guide provides detailed implementation instructions for setting up GitHub's autonomous CI/CD capabilities for the AI Partner application.

## Table of Contents

1. [GitHub App Setup](#github-app-setup)
2. [Autonomous Workflows](#autonomous-workflows)
3. [Self-Healing Actions](#self-healing-actions)
4. [Deployment Automation](#deployment-automation)
5. [Monitoring Integration](#monitoring-integration)

---

## GitHub App Setup

### Step 1: Create GitHub App

1. Navigate to: `https://github.com/settings/apps/new`

2. Configure App Settings:
```yaml
App Name: AI-Partner-Autonomous-System
Homepage URL: https://visual-x.com
Webhook URL: https://api.visual-x.com/webhooks/github
Webhook Secret: [Generate secure secret]
```

3. Set Permissions:

**Repository Permissions**:
```yaml
Actions: Read & Write
Checks: Read & Write
Contents: Read & Write
Deployments: Read & Write
Environments: Read & Write
Issues: Read & Write
Metadata: Read
Pull Requests: Read & Write
Secrets: Read & Write
Workflows: Read & Write
```

**Organization Permissions**:
```yaml
Members: Read
Secrets: Read & Write
Self-hosted runners: Read & Write
```

4. Subscribe to Events:
```yaml
- check_run
- check_suite
- deployment
- deployment_status
- issues
- pull_request
- push
- workflow_dispatch
- workflow_job
- workflow_run
```

5. Generate Private Key:
- Click "Generate private key"
- Save as `github-app-private-key.pem`
- Store in Google Secret Manager

### Step 2: Install GitHub App

```bash
# Install on organization
https://github.com/apps/YOUR-APP-NAME/installations/new

# Or via API
curl -X POST \
  https://api.github.com/app/installations/INSTALLATION_ID/access_tokens \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

### Step 3: Configure Environment

```bash
# .env.github
GITHUB_APP_ID=123456
GITHUB_APP_INSTALLATION_ID=12345678
GITHUB_PRIVATE_KEY_PATH=/secrets/github-app-private-key.pem
GITHUB_WEBHOOK_SECRET=your-webhook-secret
```

---

## Autonomous Workflows

### 1. Intelligent CI Pipeline

```yaml
# .github/workflows/intelligent-ci.yml
name: Intelligent CI

on:
  pull_request:
    types: [opened, synchronize, reopened]
  push:
    branches: [main, develop]

permissions:
  contents: read
  checks: write
  pull-requests: write
  actions: write

jobs:
  analyze-changes:
    runs-on: ubuntu-latest
    outputs:
      frontend-changed: ${{ steps.filter.outputs.frontend }}
      backend-changed: ${{ steps.filter.outputs.backend }}
      agents-changed: ${{ steps.filter.outputs.agents }}
    steps:
      - uses: actions/checkout@v4
      
      - uses: dorny/paths-filter@v2
        id: filter
        with:
          filters: |
            frontend:
              - 'frontend/**'
            backend:
              - 'backend/**'
            agents:
              - 'agents/**'

  test-frontend:
    needs: analyze-changes
    if: needs.analyze-changes.outputs.frontend-changed == 'true'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
          cache-dependency-path: frontend/package-lock.json
      
      - name: Install Dependencies
        run: cd frontend && npm ci
      
      - name: Lint
        run: cd frontend && npm run lint
      
      - name: Type Check
        run: cd frontend && npm run type-check
      
      - name: Unit Tests
        run: cd frontend && npm run test:unit
      
      - name: Integration Tests
        run: cd frontend && npm run test:integration
      
      - name: Build
        run: cd frontend && npm run build
      
      - name: Upload Coverage
        uses: codecov/codecov-action@v3
        with:
          files: ./frontend/coverage/coverage-final.json
          flags: frontend

  test-backend:
    needs: analyze-changes
    if: needs.analyze-changes.outputs.backend-changed == 'true'
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_PASSWORD: postgres
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 5432:5432
      
      redis:
        image: redis:7
        options: >-
          --health-cmd "redis-cli ping"
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 6379:6379
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.11'
          cache: 'pip'
          cache-dependency-path: backend/requirements.txt
      
      - name: Install Dependencies
        run: |
          cd backend
          pip install -r requirements.txt
          pip install -r requirements-dev.txt
      
      - name: Lint
        run: |
          cd backend
          ruff check .
          black --check .
      
      - name: Type Check
        run: cd backend && mypy .
      
      - name: Run Tests
        env:
          DATABASE_URL: postgresql://postgres:postgres@localhost:5432/test
          REDIS_URL: redis://localhost:6379
        run: |
          cd backend
          pytest --cov=app --cov-report=xml
      
      - name: Upload Coverage
        uses: codecov/codecov-action@v3
        with:
          files: ./backend/coverage.xml
          flags: backend

  test-agents:
    needs: analyze-changes
    if: needs.analyze-changes.outputs.agents-changed == 'true'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.11'
      
      - name: Install Dependencies
        run: |
          cd agents
          pip install -r requirements.txt
      
      - name: Test Agents
        run: |
          cd agents
          pytest tests/
      
      - name: Validate Agent Configs
        run: |
          cd agents
          python scripts/validate_configs.py

  security-scan:
    runs-on: ubuntu-latest
    permissions:
      security-events: write
      contents: read
    steps:
      - uses: actions/checkout@v4
      
      - name: Run CodeQL
        uses: github/codeql-action/init@v3
        with:
          languages: python, javascript
      
      - name: Autobuild
        uses: github/codeql-action/autobuild@v3
      
      - name: Perform CodeQL Analysis
        uses: github/codeql-action/analyze@v3
      
      - name: Run Trivy Security Scan
        uses: aquasecurity/trivy-action@master
        with:
          scan-type: 'fs'
          scan-ref: '.'
          format: 'sarif'
          output: 'trivy-results.sarif'
      
      - name: Upload Trivy Results
        uses: github/codeql-action/upload-sarif@v3
        with:
          sarif_file: 'trivy-results.sarif'

  auto-merge:
    needs: [test-frontend, test-backend, test-agents, security-scan]
    if: github.event_name == 'pull_request' && github.actor == 'dependabot[bot]'
    runs-on: ubuntu-latest
    permissions:
      pull-requests: write
      contents: write
    steps:
      - name: Enable Auto-merge
        run: gh pr merge --auto --squash "$PR_URL"
        env:
          PR_URL: ${{ github.event.pull_request.html_url }}
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### 2. Progressive Deployment

```yaml
# .github/workflows/progressive-deployment.yml
name: Progressive Deployment

on:
  push:
    branches: [main]
  workflow_dispatch:
    inputs:
      environment:
        description: 'Target environment'
        required: true
        type: choice
        options:
          - staging
          - production

permissions:
  contents: read
  deployments: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      
      - name: Login to GCR
        uses: docker/login-action@v3
        with:
          registry: gcr.io
          username: _json_key
          password: ${{ secrets.GCP_SERVICE_ACCOUNT_KEY }}
      
      - name: Build and Push Frontend
        uses: docker/build-push-action@v5
        with:
          context: ./frontend
          push: true
          tags: |
            gcr.io/${{ secrets.GCP_PROJECT_ID }}/frontend:${{ github.sha }}
            gcr.io/${{ secrets.GCP_PROJECT_ID }}/frontend:latest
          cache-from: type=gha
          cache-to: type=gha,mode=max
      
      - name: Build and Push Backend
        uses: docker/build-push-action@v5
        with:
          context: ./backend
          push: true
          tags: |
            gcr.io/${{ secrets.GCP_PROJECT_ID }}/backend:${{ github.sha }}
            gcr.io/${{ secrets.GCP_PROJECT_ID }}/backend:latest
      
      - name: Build and Push Agents
        uses: docker/build-push-action@v5
        with:
          context: ./agents
          push: true
          tags: |
            gcr.io/${{ secrets.GCP_PROJECT_ID }}/agents:${{ github.sha }}
            gcr.io/${{ secrets.GCP_PROJECT_ID }}/agents:latest

  deploy-staging:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: staging
      url: https://staging.visual-x.com
    steps:
      - uses: actions/checkout@v4
      
      - name: Authenticate to GCP
        uses: google-github-actions/auth@v2
        with:
          credentials_json: ${{ secrets.GCP_SERVICE_ACCOUNT_KEY }}
      
      - name: Set up Cloud SDK
        uses: google-github-actions/setup-gcloud@v2
      
      - name: Deploy to GKE Staging
        run: |
          gcloud container clusters get-credentials staging-cluster \
            --region us-central1
          
          kubectl set image deployment/frontend \
            frontend=gcr.io/${{ secrets.GCP_PROJECT_ID }}/frontend:${{ github.sha }}
          
          kubectl set image deployment/backend \
            backend=gcr.io/${{ secrets.GCP_PROJECT_ID }}/backend:${{ github.sha }}
          
          kubectl set image deployment/agents \
            agents=gcr.io/${{ secrets.GCP_PROJECT_ID }}/agents:${{ github.sha }}
          
          kubectl rollout status deployment/frontend
          kubectl rollout status deployment/backend
          kubectl rollout status deployment/agents
      
      - name: Run Smoke Tests
        run: |
          sleep 30
          npm run test:smoke -- --env=staging
      
      - name: Monitor Metrics
        uses: ./.github/actions/monitor-deployment
        with:
          environment: staging
          duration: 300

  deploy-production:
    needs: deploy-staging
    if: success()
    runs-on: ubuntu-latest
    environment:
      name: production
      url: https://visual-x.com
    steps:
      - uses: actions/checkout@v4
      
      - name: Authenticate to GCP
        uses: google-github-actions/auth@v2
        with:
          credentials_json: ${{ secrets.GCP_SERVICE_ACCOUNT_KEY }}
      
      - name: Deploy with Canary
        run: |
          # Deploy to 10% of traffic first
          kubectl apply -f k8s/canary/
          
          # Monitor for 5 minutes
          sleep 300
          
          # Check error rates
          ERROR_RATE=$(curl -s https://api.visual-x.com/metrics/error-rate)
          if [ "$ERROR_RATE" -gt 1 ]; then
            echo "Error rate too high, rolling back"
            kubectl delete -f k8s/canary/
            exit 1
          fi
          
          # Gradually increase traffic
          for i in 25 50 75 100; do
            kubectl patch service frontend -p \
              "{\"spec\":{\"selector\":{\"version\":\"canary\"},\"weight\":$i}}"
            sleep 180
          done
          
          # Finalize deployment
          kubectl apply -f k8s/production/
      
      - name: Verify Deployment
        run: |
          kubectl rollout status deployment/frontend
          kubectl rollout status deployment/backend
          kubectl rollout status deployment/agents
      
      - name: Notify Success
        uses: slackapi/slack-github-action@v1
        with:
          webhook-url: ${{ secrets.SLACK_WEBHOOK }}
          payload: |
            {
              "text": "✅ Production deployment successful",
              "blocks": [
                {
                  "type": "section",
                  "text": {
                    "type": "mrkdwn",
                    "text": "*Production Deployment*\n✅ Successfully deployed version ${{ github.sha }}"
                  }
                }
              ]
            }
```

### 3. Continuous Monitoring & Auto-Remediation

```yaml
# .github/workflows/continuous-monitoring.yml
name: Continuous Monitoring

on:
  schedule:
    - cron: '*/5 * * * *'  # Every 5 minutes
  workflow_dispatch:

permissions:
  contents: write
  actions: write
  deployments: write

jobs:
  health-monitoring:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Check System Health
        id: health
        run: |
          # Check all critical endpoints
          ENDPOINTS=(
            "https://api.visual-x.com/health"
            "https://visual-x.com"
            "https://agents.visual-x.com/health"
          )
          
          FAILED=0
          for endpoint in "${ENDPOINTS[@]}"; do
            if ! curl -f -s --max-time 10 "$endpoint" > /dev/null; then
              echo "Failed: $endpoint"
              FAILED=$((FAILED + 1))
            fi
          done
          
          echo "failed_count=$FAILED" >> $GITHUB_OUTPUT
      
      - name: Check Database Connections
        id: db
        run: |
          # Check database health
          kubectl exec -n production deploy/backend -- \
            python -c "from app.core.db import check_connection; check_connection()"
      
      - name: Check Error Rates
        id: errors
        run: |
          # Query metrics from GCP
          ERROR_RATE=$(gcloud monitoring time-series list \
            --filter='metric.type="custom.googleapis.com/error_rate"' \
            --format='value(points[0].value.doubleValue)')
          
          echo "error_rate=$ERROR_RATE" >> $GITHUB_OUTPUT
      
      - name: Check Resource Usage
        id: resources
        run: |
          # Check CPU and memory usage
          CPU=$(kubectl top pods -n production --no-headers | \
            awk '{sum+=$2} END {print sum}')
          MEMORY=$(kubectl top pods -n production --no-headers | \
            awk '{sum+=$3} END {print sum}')
          
          echo "cpu=$CPU" >> $GITHUB_OUTPUT
          echo "memory=$MEMORY" >> $GITHUB_OUTPUT
      
      - name: Trigger Auto-Remediation
        if: |
          steps.health.outputs.failed_count > 0 ||
          steps.errors.outputs.error_rate > 5 ||
          steps.resources.outputs.cpu > 80
        uses: ./.github/actions/auto-remediate
        with:
          issue_type: ${{ steps.health.outputs.failed_count > 0 && 'service_down' || 
                         steps.errors.outputs.error_rate > 5 && 'high_error_rate' ||
                         'high_resource_usage' }}
          severity: critical
      
      - name: Create Incident Report
        if: failure()
        uses: actions/github-script@v7
        with:
          script: |
            await github.rest.issues.create({
              owner: context.repo.owner,
              repo: context.repo.repo,
              title: '🚨 System Health Alert',
              body: `## Health Check Failed
              
              **Failed Endpoints**: ${{ steps.health.outputs.failed_count }}
              **Error Rate**: ${{ steps.errors.outputs.error_rate }}%
              **CPU Usage**: ${{ steps.resources.outputs.cpu }}%
              **Memory Usage**: ${{ steps.resources.outputs.memory }}%
              
              Auto-remediation attempted.`,
              labels: ['incident', 'auto-generated']
            })
```

---

## Self-Healing Actions

### Auto-Remediate Action

```yaml
# .github/actions/auto-remediate/action.yml
name: Auto-Remediate
description: Automatically remediate common issues

inputs:
  issue_type:
    description: 'Type of issue'
    required: true
  severity:
    description: 'Issue severity'
    required: true

runs:
  using: composite
  steps:
    - name: Restart Services
      if: inputs.issue_type == 'service_down'
      shell: bash
      run: |
        kubectl rollout restart deployment/frontend -n production
        kubectl rollout restart deployment/backend -n production
        kubectl rollout restart deployment/agents -n production
        
        # Wait for restart
        kubectl rollout status deployment/frontend -n production
        kubectl rollout status deployment/backend -n production
        kubectl rollout status deployment/agents -n production
    
    - name: Scale Up Resources
      if: inputs.issue_type == 'high_resource_usage'
      shell: bash
      run: |
        # Increase replicas
        kubectl scale deployment/frontend --replicas=5 -n production
        kubectl scale deployment/backend --replicas=5 -n production
        
        # Increase resource limits
        kubectl set resources deployment/frontend \
          --limits=cpu=2000m,memory=2Gi -n production
        kubectl set resources deployment/backend \
          --limits=cpu=2000m,memory=2Gi -n production
    
    - name: Clear Cache
      if: inputs.issue_type == 'high_error_rate'
      shell: bash
      run: |
        # Clear Redis cache
        kubectl exec -n production deploy/redis -- redis-cli FLUSHALL
        
        # Restart backend to clear application cache
        kubectl rollout restart deployment/backend -n production
    
    - name: Rollback Deployment
      if: inputs.severity == 'critical'
      shell: bash
      run: |
        # Rollback to previous version
        kubectl rollout undo deployment/frontend -n production
        kubectl rollout undo deployment/backend -n production
        kubectl rollout undo deployment/agents -n production
    
    - name: Notify Team
      shell: bash
      env:
        SLACK_WEBHOOK: ${{ env.SLACK_WEBHOOK }}
      run: |
        curl -X POST "$SLACK_WEBHOOK" \
          -H 'Content-Type: application/json' \
          -d "{
            \"text\": \"🔧 Auto-remediation triggered\",
            \"blocks\": [{
              \"type\": \"section\",
              \"text\": {
                \"type\": \"mrkdwn\",
                \"text\": \"*Issue*: ${{ inputs.issue_type }}\n*Severity*: ${{ inputs.severity }}\n*Action*: Automated remediation in progress\"
              }
            }]
          }"
```

### Monitor Deployment Action

```yaml
# .github/actions/monitor-deployment/action.yml
name: Monitor Deployment
description: Monitor deployment health and metrics

inputs:
  environment:
    description: 'Environment to monitor'
    required: true
  duration:
    description: 'Duration in seconds'
    required: true
    default: '300'

runs:
  using: composite
  steps:
    - name: Setup Monitoring
      shell: bash
      run: |
        # Install monitoring tools
        pip install prometheus-client requests
    
    - name: Monitor Metrics
      shell: bash
      run: |
        python << 'EOF'
        import time
        import requests
        import sys
        
        DURATION = int("${{ inputs.duration }}")
        ENV = "${{ inputs.environment }}"
        BASE_URL = f"https://{'staging.' if ENV == 'staging' else ''}visual-x.com"
        
        start_time = time.time()
        error_count = 0
        success_count = 0
        
        while time.time() - start_time < DURATION:
            try:
                # Check health endpoint
                response = requests.get(f"{BASE_URL}/health", timeout=5)
                if response.status_code == 200:
                    success_count += 1
                else:
                    error_count += 1
                    
                # Check metrics
                metrics = requests.get(f"{BASE_URL}/metrics").json()
                
                # Alert if error rate is high
                if metrics.get('error_rate', 0) > 5:
                    print(f"⚠️  High error rate: {metrics['error_rate']}%")
                    
                # Alert if latency is high
                if metrics.get('p95_latency', 0) > 2000:
                    print(f"⚠️  High latency: {metrics['p95_latency']}ms")
                    
            except Exception as e:
                error_count += 1
                print(f"❌ Health check failed: {e}")
            
            time.sleep(10)
        
        # Calculate success rate
        total = success_count + error_count
        success_rate = (success_count / total * 100) if total > 0 else 0
        
        print(f"✅ Monitoring complete")
        print(f"   Success rate: {success_rate:.2f}%")
        print(f"   Total checks: {total}")
        
        # Fail if success rate is below threshold
        if success_rate < 95:
            print(f"❌ Deployment failed: Success rate {success_rate:.2f}% < 95%")
            sys.exit(1)
        
        EOF
```

---

## Deployment Automation

### Terraform Configuration

```hcl
# infrastructure/main.tf
terraform {
  required_version = ">= 1.6"
  
  backend "gcs" {
    bucket = "ai-partner-terraform-state"
    prefix = "terraform/state"
  }
  
  required_providers {
    google = {
      source  = "hashicorp/google"
      version = "~> 5.0"
    }
    kubernetes = {
      source  = "hashicorp/kubernetes"
      version = "~> 2.24"
    }
  }
}

provider "google" {
  project = var.project_id
  region  = var.region
}

# GKE Cluster
resource "google_container_cluster" "primary" {
  name     = "ai-partner-cluster"
  location = var.region
  
  remove_default_node_pool = true
  initial_node_count       = 1
  
  workload_identity_config {
    workload_pool = "${var.project_id}.svc.id.goog"
  }
  
  addons_config {
    http_load_balancing {
      disabled = false
    }
    horizontal_pod_autoscaling {
      disabled = false
    }
  }
}

resource "google_container_node_pool" "primary_nodes" {
  name       = "primary-node-pool"
  cluster    = google_container_cluster.primary.id
  node_count = 3
  
  autoscaling {
    min_node_count = 3
    max_node_count = 10
  }
  
  node_config {
    preemptible  = false
    machine_type = "e2-standard-4"
    
    oauth_scopes = [
      "https://www.googleapis.com/auth/cloud-platform"
    ]
    
    metadata = {
      disable-legacy-endpoints = "true"
    }
  }
}

# Cloud SQL (PostgreSQL)
resource "google_sql_database_instance" "main" {
  name             = "ai-partner-db"
  database_version = "POSTGRES_15"
  region           = var.region
  
  settings {
    tier = "db-custom-4-16384"
    
    backup_configuration {
      enabled = true
      start_time = "03:00"
    }
    
    ip_configuration {
      ipv4_enabled = false
      private_network = google_compute_network.private_network.id
    }
  }
}

# Vertex AI resources
resource "google_vertex_ai_endpoint" "prediction_endpoint" {
  name         = "prediction-endpoint"
  display_name = "AI Partner Prediction Endpoint"
  region       = var.region
}
```

### GitHub Actions for Infrastructure

```yaml
# .github/workflows/infrastructure.yml
name: Infrastructure Management

on:
  push:
    paths:
      - 'infrastructure/**'
    branches: [main]
  workflow_dispatch:
    inputs:
      action:
        description: 'Terraform action'
        required: true
        type: choice
        options:
          - plan
          - apply
          - destroy

permissions:
  contents: read
  id-token: write

jobs:
  terraform:
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: infrastructure
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v3
        with:
          terraform_version: 1.6.0
      
      - name: Authenticate to GCP
        uses: google-github-actions/auth@v2
        with:
          workload_identity_provider: ${{ secrets.WIF_PROVIDER }}
          service_account: ${{ secrets.WIF_SERVICE_ACCOUNT }}
      
      - name: Terraform Init
        run: terraform init
      
      - name: Terraform Plan
        id: plan
        run: terraform plan -no-color -out=tfplan
        continue-on-error: true
      
      - name: Comment Plan on PR
        if: github.event_name == 'pull_request'
        uses: actions/github-script@v7
        with:
          script: |
            const output = `#### Terraform Plan 📖
            \`\`\`
            ${{ steps.plan.outputs.stdout }}
            \`\`\`
            `;
            
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: output
            })
      
      - name: Terraform Apply
        if: github.event_name == 'push' || github.event.inputs.action == 'apply'
        run: terraform apply -auto-approve tfplan
```

---

## Monitoring Integration

### Prometheus Configuration

```yaml
# monitoring/prometheus.yml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

alerting:
  alertmanagers:
    - static_configs:
        - targets:
            - alertmanager:9093

rule_files:
  - "alerts.yml"

scrape_configs:
  - job_name: 'kubernetes-pods'
    kubernetes_sd_configs:
      - role: pod
    relabel_configs:
      - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_scrape]
        action: keep
        regex: true
```

### Alert Rules

```yaml
# monitoring/alerts.yml
groups:
  - name: application
    interval: 30s
    rules:
      - alert: HighErrorRate
        expr: rate(http_requests_total{status=~"5.."}[5m]) > 0.05
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "High error rate detected"
          description: "Error rate is {{ $value }}% for {{ $labels.instance }}"
      
      - alert: HighLatency
        expr: histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m])) > 2
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "High latency detected"
          description: "95th percentile latency is {{ $value }}s"
      
      - alert: ServiceDown
        expr: up == 0
        for: 1m
        labels:
          severity: critical
        annotations:
          summary: "Service is down"
          description: "{{ $labels.job }} is down"
```

---

## Best Practices

### 1. Secret Management

```bash
# Store secrets in Google Secret Manager
gcloud secrets create github-webhook-secret \
  --data-file=- <<< "your-secret-value"

# Grant access to service account
gcloud secrets add-iam-policy-binding github-webhook-secret \
  --member="serviceAccount:github-actions@project.iam.gserviceaccount.com" \
  --role="roles/secretmanager.secretAccessor"
```

### 2. Workflow Security

- Always use `permissions:` block
- Pin action versions with SHA
- Use `GITHUB_TOKEN` with minimum permissions
- Store credentials in secrets
- Enable branch protection rules

### 3. Monitoring Checklist

- [ ] Application metrics exposed
- [ ] Logs centralized
- [ ] Alerts configured
- [ ] Dashboards created
- [ ] SLO/SLA defined
- [ ] Incident response procedures

### 4. Deployment Checklist

- [ ] Health checks configured
- [ ] Graceful shutdown implemented
- [ ] Database migrations automated
- [ ] Rollback procedure tested
- [ ] Blue-green deployment ready
- [ ] Canary deployment configured

---

## Troubleshooting

### Common Issues

**Issue**: Workflow fails with permission error
```bash
# Solution: Add required permissions
permissions:
  contents: read
  checks: write
```

**Issue**: Deployment timeout
```bash
# Solution: Increase timeout and check pod status
kubectl get pods -n production
kubectl describe pod <pod-name> -n production
kubectl logs <pod-name> -n production
```

**Issue**: High error rate after deployment
```bash
# Solution: Immediate rollback
kubectl rollout undo deployment/backend -n production
```

---

## Next Steps

1. Set up GitHub App with required permissions
2. Configure webhooks and secret management
3. Deploy infrastructure with Terraform
4. Set up monitoring and alerting
5. Test autonomous workflows
6. Configure self-healing actions
7. Document runbooks and procedures

---

**Document Version**: 1.0  
**Last Updated**: 2026-02-05  
**Maintained By**: DevOps Team
