# Security Policy

## Reporting Vulnerabilities

Security is taken seriously. If you identify a potential security vulnerability within this project, please report it responsibly:

- Submit a private vulnerability advisory or report directly to the repository maintainer.
- Please allow reasonable time for investigation and remediation before public disclosure.
- Provide descriptive reproduction steps or a minimal proof of concept.

## Security Practices

This project implements standard enterprise security safeguards:

- **Data Protection**: Parameterized queries via GORM prevent SQL injection; DTOs and value objects sanitize inputs.
- **Transport & Edge Security**: Request timeout enforcement, payload size limits (1MB default), CORS policy configuration, and standard security headers.
- **Container Hardening**: Multi-stage Docker builds running under an unprivileged user (`appuser`), read-only root filesystem, dropped Linux capabilities (`ALL`), and `no-new-privileges` enabled.
- **Credential Hygiene**: Sensitive configurations are injected via environment variables; local `.env` files are strictly excluded from source control.

