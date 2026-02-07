# Security Policy

## Supported Versions

We release patches for security vulnerabilities. Which versions are eligible for receiving such patches depends on the CVSS v3.0 Rating:

| Version | Supported          |
| ------- | ------------------ |
| 1.x.x   | :white_check_mark: |
| < 1.0   | :x:                |

## Reporting a Vulnerability

We take the security of git-ops seriously. If you have discovered a security vulnerability, we appreciate your help in disclosing it to us in a responsible manner.

### Please Do:

1. **Report via GitHub Security Advisories**: Use the [GitHub Security Advisories](https://github.com/InfinityXOneSystems/git-ops/security/advisories/new) feature to privately report vulnerabilities
2. **Provide Detailed Information**: Include as much information as possible:
   - Type of vulnerability
   - Steps to reproduce or proof-of-concept
   - Potential impact
   - Suggested fix (if any)
3. **Allow Time for Fix**: Give us reasonable time to address the issue before public disclosure

### Please Don't:

- Publicly disclose the vulnerability before it has been addressed
- Access or modify data that doesn't belong to you
- Perform actions that could negatively affect our users or services

## Response Timeline

- **Initial Response**: Within 48 hours of report submission
- **Status Update**: Within 7 days with an assessment of the report
- **Fix Development**: Depends on severity and complexity
- **Public Disclosure**: After fix is deployed and users have had time to update

## Security Update Process

1. Vulnerability reported and confirmed
2. Security patch developed and tested
3. Security advisory published
4. Patch released and users notified
5. Public disclosure (if appropriate)

## Security Best Practices

When using git-ops, we recommend:

- Always use the latest stable version
- Regularly update dependencies
- Follow the principle of least privilege
- Use environment variables for sensitive data
- Enable two-factor authentication on your GitHub account
- Review and audit third-party dependencies
- Use secrets management tools for sensitive credentials

## Recognition

We appreciate the security research community and believe in recognizing those who help us keep our project secure. Reporters of valid security issues will be:

- Credited in our security advisories (if desired)
- Mentioned in our changelog and release notes
- Added to our security acknowledgments page

## Contact

For any security-related questions or concerns, please use GitHub Security Advisories or contact the maintainers directly.

## Scope

This security policy applies to:

- The git-ops repository
- Official releases and distributions
- Dependencies we directly maintain

## Additional Resources

- [GitHub Security Best Practices](https://docs.github.com/en/code-security)
- [OWASP Security Guidelines](https://owasp.org/)
- [CWE/SANS Top 25](https://cwe.mitre.org/top25/)

Thank you for helping keep git-ops and its users safe!
