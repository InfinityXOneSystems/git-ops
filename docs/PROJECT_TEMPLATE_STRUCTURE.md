# Project Template Structure

## Complete Directory Structure

```
ai-partner-app/
├── .github/
│   ├── actions/
│   │   ├── auto-remediate/
│   │   │   ├── action.yml
│   │   │   └── remediate.sh
│   │   ├── monitor-deployment/
│   │   │   ├── action.yml
│   │   │   └── monitor.py
│   │   └── setup-env/
│   │       └── action.yml
│   ├── workflows/
│   │   ├── ci.yml
│   │   ├── deploy.yml
│   │   ├── self-healing.yml
│   │   ├── infrastructure.yml
│   │   ├── security-scan.yml
│   │   └── build-agent.yml
│   ├── dependabot.yml
│   ├── CODEOWNERS
│   └── ISSUE_TEMPLATE/
│       ├── bug_report.md
│       ├── feature_request.md
│       └── incident_report.md
│
├── frontend/
│   ├── app/
│   │   ├── (auth)/
│   │   │   ├── login/
│   │   │   │   └── page.tsx
│   │   │   └── register/
│   │   │       └── page.tsx
│   │   ├── (dashboard)/
│   │   │   ├── layout.tsx
│   │   │   ├── page.tsx
│   │   │   ├── agents/
│   │   │   │   ├── page.tsx
│   │   │   │   ├── [id]/
│   │   │   │   │   ├── page.tsx
│   │   │   │   │   └── edit/
│   │   │   │   │       └── page.tsx
│   │   │   │   └── new/
│   │   │   │       └── page.tsx
│   │   │   ├── editor/
│   │   │   │   └── page.tsx
│   │   │   ├── predictions/
│   │   │   │   └── page.tsx
│   │   │   ├── monitoring/
│   │   │   │   └── page.tsx
│   │   │   └── settings/
│   │   │       └── page.tsx
│   │   ├── api/
│   │   │   ├── auth/
│   │   │   │   └── [...nextauth]/
│   │   │   │       └── route.ts
│   │   │   └── webhooks/
│   │   │       └── route.ts
│   │   ├── layout.tsx
│   │   ├── page.tsx
│   │   └── globals.css
│   ├── components/
│   │   ├── ui/
│   │   │   ├── button.tsx
│   │   │   ├── card.tsx
│   │   │   ├── dialog.tsx
│   │   │   ├── dropdown-menu.tsx
│   │   │   ├── input.tsx
│   │   │   ├── label.tsx
│   │   │   ├── select.tsx
│   │   │   ├── table.tsx
│   │   │   ├── tabs.tsx
│   │   │   └── toast.tsx
│   │   ├── editor/
│   │   │   ├── monaco-editor.tsx
│   │   │   ├── editor-toolbar.tsx
│   │   │   ├── file-tree.tsx
│   │   │   └── terminal.tsx
│   │   ├── agents/
│   │   │   ├── agent-card.tsx
│   │   │   ├── agent-builder.tsx
│   │   │   ├── agent-list.tsx
│   │   │   ├── agent-config-form.tsx
│   │   │   └── agent-status-badge.tsx
│   │   ├── monitoring/
│   │   │   ├── metrics-chart.tsx
│   │   │   ├── health-indicator.tsx
│   │   │   ├── activity-feed.tsx
│   │   │   └── alert-panel.tsx
│   │   └── layout/
│   │       ├── header.tsx
│   │       ├── sidebar.tsx
│   │       ├── footer.tsx
│   │       └── theme-provider.tsx
│   ├── lib/
│   │   ├── api.ts
│   │   ├── socket.ts
│   │   ├── auth.ts
│   │   ├── utils.ts
│   │   └── constants.ts
│   ├── hooks/
│   │   ├── use-editor.ts
│   │   ├── use-agents.ts
│   │   ├── use-realtime.ts
│   │   ├── use-auth.ts
│   │   └── use-toast.ts
│   ├── types/
│   │   ├── agent.ts
│   │   ├── editor.ts
│   │   ├── api.ts
│   │   └── index.ts
│   ├── public/
│   │   ├── favicon.ico
│   │   └── images/
│   ├── __tests__/
│   │   ├── components/
│   │   ├── pages/
│   │   └── integration/
│   ├── .env.local.example
│   ├── .eslintrc.json
│   ├── .prettierrc
│   ├── Dockerfile
│   ├── next.config.js
│   ├── package.json
│   ├── tailwind.config.ts
│   └── tsconfig.json
│
├── backend/
│   ├── app/
│   │   ├── api/
│   │   │   ├── v1/
│   │   │   │   ├── endpoints/
│   │   │   │   │   ├── __init__.py
│   │   │   │   │   ├── agents.py
│   │   │   │   │   ├── auth.py
│   │   │   │   │   ├── editor.py
│   │   │   │   │   ├── health.py
│   │   │   │   │   ├── predictions.py
│   │   │   │   │   ├── scraping.py
│   │   │   │   │   ├── users.py
│   │   │   │   │   └── webhooks.py
│   │   │   │   ├── __init__.py
│   │   │   │   └── router.py
│   │   │   ├── __init__.py
│   │   │   └── deps.py
│   │   ├── core/
│   │   │   ├── __init__.py
│   │   │   ├── config.py
│   │   │   ├── security.py
│   │   │   ├── auth.py
│   │   │   ├── db.py
│   │   │   └── celery_app.py
│   │   ├── models/
│   │   │   ├── __init__.py
│   │   │   ├── agent.py
│   │   │   ├── user.py
│   │   │   ├── task.py
│   │   │   ├── prediction.py
│   │   │   └── scraping_job.py
│   │   ├── schemas/
│   │   │   ├── __init__.py
│   │   │   ├── agent.py
│   │   │   ├── user.py
│   │   │   ├── task.py
│   │   │   └── common.py
│   │   ├── services/
│   │   │   ├── __init__.py
│   │   │   ├── vertex_ai.py
│   │   │   ├── orchestrator.py
│   │   │   ├── sandbox.py
│   │   │   ├── healing.py
│   │   │   ├── scraper.py
│   │   │   └── cache.py
│   │   ├── middleware/
│   │   │   ├── __init__.py
│   │   │   ├── security.py
│   │   │   ├── logging.py
│   │   │   └── rate_limit.py
│   │   ├── workers/
│   │   │   ├── __init__.py
│   │   │   ├── tasks.py
│   │   │   └── periodic.py
│   │   ├── websockets/
│   │   │   ├── __init__.py
│   │   │   ├── connection_manager.py
│   │   │   └── handlers.py
│   │   ├── utils/
│   │   │   ├── __init__.py
│   │   │   ├── helpers.py
│   │   │   └── validators.py
│   │   └── main.py
│   ├── alembic/
│   │   ├── versions/
│   │   ├── env.py
│   │   └── script.py.mako
│   ├── tests/
│   │   ├── conftest.py
│   │   ├── test_api/
│   │   ├── test_services/
│   │   └── test_models/
│   ├── scripts/
│   │   ├── init_db.py
│   │   └── seed_data.py
│   ├── .env.example
│   ├── .coveragerc
│   ├── alembic.ini
│   ├── Dockerfile
│   ├── pyproject.toml
│   ├── requirements.txt
│   └── requirements-dev.txt
│
├── agents/
│   ├── base/
│   │   ├── __init__.py
│   │   ├── agent.py
│   │   └── validator.py
│   ├── scraping/
│   │   ├── __init__.py
│   │   ├── agent.py
│   │   ├── config.py
│   │   └── parsers.py
│   ├── prediction/
│   │   ├── __init__.py
│   │   ├── agent.py
│   │   ├── config.py
│   │   └── models.py
│   ├── monitoring/
│   │   ├── __init__.py
│   │   ├── agent.py
│   │   ├── config.py
│   │   └── collectors.py
│   ├── healing/
│   │   ├── __init__.py
│   │   ├── agent.py
│   │   ├── config.py
│   │   └── strategies.py
│   ├── builder/
│   │   ├── __init__.py
│   │   ├── agent.py
│   │   ├── config.py
│   │   └── templates/
│   ├── orchestrator/
│   │   ├── __init__.py
│   │   ├── manager.py
│   │   ├── scheduler.py
│   │   └── router.py
│   ├── shared/
│   │   ├── __init__.py
│   │   ├── utils.py
│   │   └── constants.py
│   ├── tests/
│   │   ├── test_agents/
│   │   └── test_orchestrator/
│   ├── configs/
│   │   ├── scraping_templates.yml
│   │   ├── prediction_templates.yml
│   │   └── monitoring_templates.yml
│   ├── Dockerfile
│   ├── requirements.txt
│   └── setup.py
│
├── infrastructure/
│   ├── modules/
│   │   ├── gke-cluster/
│   │   │   ├── main.tf
│   │   │   ├── variables.tf
│   │   │   └── outputs.tf
│   │   ├── cloud-sql/
│   │   │   ├── main.tf
│   │   │   ├── variables.tf
│   │   │   └── outputs.tf
│   │   ├── redis/
│   │   │   ├── main.tf
│   │   │   ├── variables.tf
│   │   │   └── outputs.tf
│   │   ├── networking/
│   │   │   ├── main.tf
│   │   │   ├── variables.tf
│   │   │   └── outputs.tf
│   │   └── vertex-ai/
│   │       ├── main.tf
│   │       ├── variables.tf
│   │       └── outputs.tf
│   ├── environments/
│   │   ├── dev/
│   │   │   ├── main.tf
│   │   │   ├── variables.tf
│   │   │   └── terraform.tfvars
│   │   ├── staging/
│   │   │   ├── main.tf
│   │   │   ├── variables.tf
│   │   │   └── terraform.tfvars
│   │   └── production/
│   │       ├── main.tf
│   │       ├── variables.tf
│   │       └── terraform.tfvars
│   ├── scripts/
│   │   ├── init.sh
│   │   └── destroy.sh
│   ├── .terraform.lock.hcl
│   ├── backend.tf
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
│
├── k8s/
│   ├── base/
│   │   ├── namespace.yml
│   │   ├── configmap.yml
│   │   └── secrets.yml
│   ├── deployments/
│   │   ├── frontend.yml
│   │   ├── backend.yml
│   │   ├── agents.yml
│   │   ├── redis.yml
│   │   └── postgres.yml
│   ├── services/
│   │   ├── frontend-service.yml
│   │   ├── backend-service.yml
│   │   └── agents-service.yml
│   ├── ingress/
│   │   └── ingress.yml
│   ├── hpa/
│   │   ├── frontend-hpa.yml
│   │   ├── backend-hpa.yml
│   │   └── agents-hpa.yml
│   ├── pdb/
│   │   └── pdb.yml
│   ├── jobs/
│   │   └── migrations.yml
│   ├── cronjobs/
│   │   └── cleanup.yml
│   └── monitoring/
│       ├── servicemonitor.yml
│       └── prometheusrule.yml
│
├── monitoring/
│   ├── prometheus/
│   │   ├── prometheus.yml
│   │   ├── alerts.yml
│   │   └── rules.yml
│   ├── grafana/
│   │   ├── dashboards/
│   │   │   ├── system-overview.json
│   │   │   ├── application-metrics.json
│   │   │   ├── agent-metrics.json
│   │   │   └── database-metrics.json
│   │   ├── datasources.yml
│   │   └── grafana.ini
│   ├── alertmanager/
│   │   └── alertmanager.yml
│   └── loki/
│       └── loki.yml
│
├── scripts/
│   ├── init-project.sh
│   ├── setup-gcp.sh
│   ├── deploy.sh
│   ├── rollback.sh
│   ├── backup.sh
│   └── cleanup.sh
│
├── docs/
│   ├── AUTONOMOUS_AI_SYSTEM_HANDOFF.md
│   ├── GITHUB_AUTONOMOUS_CICD.md
│   ├── COPILOT_IMPLEMENTATION_GUIDE.md
│   ├── API_REFERENCE.md
│   ├── USER_GUIDE.md
│   ├── DEPLOYMENT_GUIDE.md
│   ├── TROUBLESHOOTING.md
│   └── ARCHITECTURE.md
│
├── tests/
│   ├── e2e/
│   │   ├── playwright.config.ts
│   │   └── tests/
│   ├── load/
│   │   ├── k6-config.js
│   │   └── scenarios/
│   └── integration/
│       └── api-tests.py
│
├── .editorconfig
├── .env.example
├── .gitignore
├── .dockerignore
├── docker-compose.yml
├── docker-compose.dev.yml
├── docker-compose.test.yml
├── Makefile
├── README.md
├── CONTRIBUTING.md
├── LICENSE
└── package.json (root)
```

## Key Configuration Files

### Root package.json

```json
{
  "name": "ai-partner-app",
  "version": "1.0.0",
  "private": true,
  "workspaces": [
    "frontend"
  ],
  "scripts": {
    "dev": "docker-compose -f docker-compose.dev.yml up",
    "build": "npm run build --workspace=frontend",
    "test": "npm run test --workspace=frontend && pytest backend/tests",
    "test:e2e": "playwright test",
    "test:load": "k6 run tests/load/scenarios/smoke.js",
    "lint": "npm run lint --workspace=frontend && ruff check backend/",
    "format": "npm run format --workspace=frontend && black backend/",
    "deploy:staging": "kubectl apply -k k8s/overlays/staging",
    "deploy:production": "kubectl apply -k k8s/overlays/production"
  }
}
```

### Makefile

```makefile
.PHONY: help install dev build test deploy clean

help: ## Show this help
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-20s\033[0m %s\n", $$1, $$2}'

install: ## Install all dependencies
	npm install
	cd frontend && npm install
	cd backend && pip install -r requirements.txt
	cd agents && pip install -r requirements.txt

dev: ## Start development environment
	docker-compose -f docker-compose.dev.yml up

build: ## Build all services
	docker-compose build

test: ## Run all tests
	npm run test:unit --workspace=frontend
	pytest backend/tests
	pytest agents/tests

test-e2e: ## Run E2E tests
	npm run test:e2e

lint: ## Lint all code
	npm run lint --workspace=frontend
	ruff check backend/
	black --check backend/

format: ## Format all code
	npm run format --workspace=frontend
	black backend/
	ruff check --fix backend/

deploy-staging: ## Deploy to staging
	kubectl apply -k k8s/overlays/staging

deploy-prod: ## Deploy to production
	kubectl apply -k k8s/overlays/production

clean: ## Clean build artifacts
	rm -rf frontend/.next frontend/out
	find . -type d -name __pycache__ -exec rm -rf {} +
	find . -type d -name .pytest_cache -exec rm -rf {} +
	find . -type f -name "*.pyc" -delete

init-gcp: ## Initialize GCP project
	./scripts/setup-gcp.sh

init-infra: ## Initialize infrastructure
	cd infrastructure && terraform init && terraform apply

backup: ## Backup database
	./scripts/backup.sh

rollback: ## Rollback deployment
	./scripts/rollback.sh
```

### docker-compose.yml

```yaml
version: '3.8'

services:
  frontend:
    build:
      context: ./frontend
      target: development
    ports:
      - "3000:3000"
    environment:
      - NEXT_PUBLIC_API_URL=http://backend:8000
      - NEXT_PUBLIC_WS_URL=ws://backend:8000/ws
    volumes:
      - ./frontend:/app
      - /app/node_modules
    depends_on:
      - backend

  backend:
    build:
      context: ./backend
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://postgres:postgres@db:5432/aipartner
      - REDIS_URL=redis://redis:6379
      - GOOGLE_APPLICATION_CREDENTIALS=/secrets/gcp-credentials.json
    volumes:
      - ./backend:/app
      - ./secrets:/secrets
    depends_on:
      - db
      - redis

  agents:
    build:
      context: ./agents
    environment:
      - BACKEND_URL=http://backend:8000
      - REDIS_URL=redis://redis:6379
    volumes:
      - ./agents:/app
    depends_on:
      - backend
      - redis

  db:
    image: postgres:15
    environment:
      - POSTGRES_DB=aipartner
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  redis:
    image: redis:7
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data

  prometheus:
    image: prom/prometheus:latest
    volumes:
      - ./monitoring/prometheus:/etc/prometheus
      - prometheus_data:/prometheus
    ports:
      - "9090:9090"

  grafana:
    image: grafana/grafana:latest
    volumes:
      - ./monitoring/grafana:/etc/grafana/provisioning
      - grafana_data:/var/lib/grafana
    ports:
      - "3001:3000"
    depends_on:
      - prometheus

volumes:
  postgres_data:
  redis_data:
  prometheus_data:
  grafana_data:
```

### .gitignore

```gitignore
# Dependencies
node_modules/
venv/
__pycache__/
*.pyc
.pytest_cache/

# Environment
.env
.env.local
.env.*.local
*.pem
secrets/

# Build outputs
.next/
out/
dist/
build/
*.egg-info/
target/

# IDE
.vscode/
.idea/
*.swp
*.swo
*~
.DS_Store

# Terraform
*.tfstate
*.tfstate.*
.terraform/
.terraform.lock.hcl

# Logs
logs/
*.log
npm-debug.log*

# Testing
coverage/
.nyc_output/
htmlcov/
.coverage

# Temporary
tmp/
temp/
*.tmp

# OS
.DS_Store
Thumbs.db
```

---

**Document Version**: 1.0  
**Last Updated**: 2026-02-05  
**Purpose**: Complete project structure reference
