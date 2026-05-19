# Security Policy

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| 1.x     | ✅ Active development |

## Reporting a Vulnerability

This project implements a DevSecOps pipeline. If you discover a vulnerability in the pipeline tooling, configuration, or associated infrastructure, please report it confidentially.

**Please do NOT report security vulnerabilities through public GitHub issues.**

Report via email to: **security@devsecops-pipeline.dev**

You should receive a response within 48 hours. If not, please follow up.

### What to Include

- Description of the vulnerability and its potential impact
- Affected component (GitHub Actions workflow, Trivy config, Semgrep rules, Checkov policies, DR scripts, etc.)
- Steps to reproduce
- Suggested remediation (if known)

## Security Practices

### Pipeline Security

- **CI/CD Hardening:** GitHub Actions workflows use pinned action versions with commit SHAs (not version tags) to prevent supply chain attacks
- **Minimal Permissions:** Workflow tokens and OIDC roles have the minimum permissions required for each job. No broad-scoped tokens
- **No Secrets in Logs:** All secrets and sensitive outputs are masked in GitHub Actions logs. Build logs are sanitized
- **Environment Isolation:** Separate environments (dev, staging, production) with distinct access controls and approval gates

### Scanning Tool Security

| Tool | Purpose | Security Measure |
|------|---------|-----------------|
| **Semgrep** | SAST - finds code-level vulnerabilities | Rules are vetted and pinned to specific versions. Custom rules are peer-reviewed |
| **GitLeaks** | Scans for secrets in code | Pre-configured with extensive secret patterns. Results are never logged in plaintext |
| **Trivy** | Container image and filesystem scanning | Database is updated before each scan. Cache is secured |
| **OWASP Dependency-Check** | SCA - dependency vulnerability scanning | NVD feed is updated regularly. False positives are documented |
| **Checkov** | IaC security scanning | 1,000+ policies enforced. Custom skip reasons must be approved |

### Quality Gate Enforcement

- **Fail on Critical/High:** Any CRITICAL or HIGH severity finding blocks the pipeline. No exceptions without formal risk acceptance
- **Audit Trail:** Every scan result is logged and retained. Pipeline passes and failures are tracked for compliance
- **Manual Override:** Bypassing a quality gate requires written approval from the security lead and is logged

### Disaster Recovery Security

- **Backup Encryption:** All backups (RDS snapshots, S3 replication) are encrypted at rest using AWS KMS
- **Cross-Region Transfer:** Data replicated across regions is encrypted in transit using TLS
- **Access Control:** Backup and restore operations require elevated IAM permissions with MFA
- **Runbook Security:** DR runbooks are stored securely and accessed only by authorized personnel

### Compliance Alignment

This pipeline maps to controls from:

- **SOC 2** (CC6.1 — Logical and Physical Access, CC7.1 — Monitoring)
- **HIPAA** (164.312 — Access Controls, Audit Controls, Integrity Controls)
- **PCI-DSS** (Requirement 6 — Secure Coding, Requirement 11 — Regular Testing)
- **NIST SP 800-53** (SI-2 — Flaw Remediation, RA-5 — Vulnerability Scanning)

## Responsible Disclosure

We request:

- Allow reasonable time (90 days) for investigation and remediation
- Do not access, modify, or exfiltrate data beyond what is necessary to demonstrate the vulnerability
- Do not perform destructive tests (denial of service, data destruction)

## Recognition

Valid vulnerability reporters will be acknowledged in our security hall of fame (with permission).
