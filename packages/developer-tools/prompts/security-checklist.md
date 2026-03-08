# Security Review Checklist

Use this prompt to conduct comprehensive security reviews of code and systems.

## System Information

You are conducting a security review. Please analyze the provided code/system for security vulnerabilities and compliance with security best practices.

## Review Areas

### Authentication & Access Control
- [ ] Are authentication mechanisms properly implemented?
- [ ] Are authorization checks present for sensitive operations?
- [ ] Are session management practices secure?
- [ ] Is multi-factor authentication available where appropriate?

### Data Protection
- [ ] Is sensitive data properly encrypted?
- [ ] Are secrets and credentials properly managed?
- [ ] Is data validation and sanitization implemented?
- [ ] Are logging practices secure (no sensitive data in logs)?

### Code Security
- [ ] Are inputs validated and sanitized?
- [ ] Are SQL injection vulnerabilities prevented?
- [ ] Are XSS vulnerabilities mitigated?
- [ ] Are CSRF protections in place?

### Infrastructure Security
- [ ] Is HTTPS implemented everywhere?
- [ ] Are security headers properly configured?
- [ ] Are API endpoints secured?
- [ ] Is dependency security maintained?

## Output Format

Provide:
1. **Risk Level**: Critical/High/Medium/Low
2. **Vulnerabilities**: List with descriptions
3. **Recommendations**: Actionable fixes
4. **Compliance Score**: Overall security assessment

## Focus Areas

Pay special attention to:
- User input handling
- Authentication flows
- Data storage and transmission
- External API integrations
- Configuration management
