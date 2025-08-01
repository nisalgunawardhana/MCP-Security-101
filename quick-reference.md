# MCP Security Quick Reference Guide

## 🚨 Critical Security Risks - Quick Overview

| Risk | Severity | Primary Impact | Quick Detection | Immediate Action |
|------|----------|----------------|-----------------|------------------|
| **Prompt Injection** | Critical | System override, data theft | Monitor for instruction patterns | Input sanitization, context isolation |
| **Tool Poisoning** | Critical | Persistent backdoors | Tool integrity checks | Code signing, sandboxing |
| **Dynamic Tool Changes** | High | Runtime compromise | Change monitoring | Immutable configs, approval workflows |
| **Misconfigured Auth** | Critical | Unauthorized access | Failed auth monitoring | MFA, external identity providers |
| **Excessive Permissions** | High | Privilege escalation | Permission audits | RBAC, least privilege |
| **Indirect Injection** | High | Stealth attacks | External data scanning | Prompt Shields, content filtering |
| **Session Hijacking** | High | User impersonation | Session pattern analysis | Secure session IDs, user binding |
| **Confused Deputy** | Medium | Authorization bypass | OAuth flow monitoring | Explicit consent, PKCE |
| **Token Passthrough** | Critical | Security control bypass | Token audience analysis | Proper token validation |
| **Supply Chain** | High | Backdoor installation | Dependency scanning | Component verification, GitHub Advanced Security |

---

## 🔍 Quick Detection Checklist

### Immediate Red Flags
- [ ] User input directly concatenated with system prompts
- [ ] `eval()`, `new Function()`, or dynamic code execution with user input
- [ ] Hardcoded credentials or API keys in source code
- [ ] All users getting admin/excessive permissions
- [ ] No session expiration or validation
- [ ] Sessions used for authentication instead of authorization verification
- [ ] Token passthrough without proper validation
- [ ] External data loaded without validation
- [ ] Missing input sanitization or validation
- [ ] Tools executable without permission checks
- [ ] Static client IDs without explicit consent workflows
- [ ] OAuth flows without PKCE implementation

### Security Scan Points
- [ ] Authentication mechanisms and session management
- [ ] Token validation and audience verification
- [ ] Input validation and sanitization routines
- [ ] Permission and access control implementations
- [ ] External data source integrations
- [ ] Tool registration and execution processes
- [ ] OAuth implementation and consent flows
- [ ] Session ID generation and binding
- [ ] Error handling and information disclosure
- [ ] Logging and monitoring capabilities
- [ ] Supply chain component verification

---

## 🛡️ Quick Prevention Measures

### Immediate Implementation
1. **Input Validation**: Validate ALL user inputs against strict schemas
2. **Output Sanitization**: Filter all outputs for sensitive information
3. **Authentication**: Implement MFA and delegate to external identity providers
4. **Permissions**: Apply principle of least privilege everywhere
5. **Code Safety**: Never execute user-provided code directly
6. **External Data**: Validate and sanitize all external data sources
7. **Token Security**: Only accept tokens explicitly issued for your MCP server
8. **Session Security**: Use secure, non-deterministic session IDs bound to users
9. **Prompt Protection**: Implement Microsoft Prompt Shields
10. **Supply Chain**: Verify all AI components and dependencies

### Essential Security Controls
```javascript
// Quick security patterns (reference only)

// 1. Input Validation
function validateInput(input) {
    if (!input || typeof input !== 'string') return false;
    if (input.length > MAX_LENGTH) return false;
    if (DANGEROUS_PATTERNS.some(pattern => pattern.test(input))) return false;
    return true;
}

// 2. Permission Checking
function hasPermission(user, action, resource) {
    const userRole = getUserRole(user);
    const requiredPermissions = getRequiredPermissions(action, resource);
    return userRole.permissions.includes(...requiredPermissions);
}

// 3. Safe Tool Execution
function executeToolSafely(toolName, params, user) {
    if (!validateToolRequest(toolName, params, user)) {
        throw new SecurityError('Invalid tool request');
    }
    
    const tool = getVerifiedTool(toolName);
    return tool.execute(sanitizeParams(params));
}

// 4. Secure Session Management
function createSecureSession(userId) {
    const sessionId = generateSecureRandomId();
    const boundSessionId = `${userId}:${sessionId}`;
    
    return {
        id: boundSessionId,
        userId: userId,
        created: Date.now(),
        expires: Date.now() + SESSION_TIMEOUT
    };
}

// 5. Token Validation
function validateMCPToken(token) {
    const decoded = jwt.verify(token, publicKey);
    
    if (decoded.aud !== MCP_SERVER_AUDIENCE) {
        throw new SecurityError('Invalid token audience');
    }
    
    return decoded;
}
```

---

## 🛡️ Microsoft Security Solutions Quick Reference

### Prompt Shields
```javascript
// Integrate Azure Prompt Shields
const promptShield = new AzurePromptShield({
    endpoint: process.env.AZURE_CONTENT_SAFETY_ENDPOINT,
    apiKey: process.env.AZURE_CONTENT_SAFETY_KEY
});

async function validateWithPromptShield(input) {
    const result = await promptShield.analyzePrompt(input);
    if (result.isInjectionDetected) {
        throw new SecurityError('Prompt injection detected');
    }
    return result.sanitizedInput;
}
```

### Azure Content Safety
- **Jailbreak Detection**: Identify bypass attempts
- **Harmful Content Screening**: Filter inappropriate content
- **Custom Policies**: Define organization-specific rules
- **Real-time Analysis**: Immediate content evaluation

### GitHub Advanced Security
- **Secret Scanning**: Detect exposed credentials
- **Dependency Scanning**: Find vulnerable components
- **CodeQL Analysis**: Static security analysis
- **Supply Chain Monitoring**: Track AI component security

---

## 🚨 Incident Response Quick Actions

### If You Detect an Attack
1. **Immediately**: Isolate affected systems
2. **Assess**: Determine scope and impact
3. **Contain**: Prevent further compromise
4. **Investigate**: Understand attack vector
5. **Remediate**: Fix vulnerabilities
6. **Monitor**: Watch for persistence or reoccurrence

### Emergency Contacts
- Security Team: [emergency contact]
- System Administrators: [contact info]
- Vendor Support: [vendor contacts]
- Legal/Compliance: [legal contacts]

---

## 📋 Security Assessment Template

### Quick Security Review
```markdown
## System: [MCP Implementation Name]
## Date: [Assessment Date]
## Reviewer: [Name/Team]

### Authentication & Authorization
- [ ] MFA implemented
- [ ] Session management secure
- [ ] RBAC properly configured
- [ ] Default credentials changed

### Input Validation
- [ ] All inputs validated
- [ ] Injection patterns blocked
- [ ] Parameter sanitization active
- [ ] Length limits enforced

### Tool Security
- [ ] Tools properly signed
- [ ] Execution sandboxed
- [ ] Permissions validated
- [ ] Registration controlled

### External Integrations
- [ ] Data sources validated
- [ ] Content filtered
- [ ] Sources allowlisted
- [ ] Integrity checked

### Monitoring & Logging
- [ ] Security events logged
- [ ] Anomaly detection active
- [ ] Alert systems configured
- [ ] Incident response ready

### Risk Rating: [Low/Medium/High/Critical]
### Key Issues: [List top 3 issues]
### Next Actions: [Immediate steps needed]
```

---

## 🔧 Essential Tools & Resources

### Security Assessment Tools
- **Static Analysis**: ESLint with security rules, SonarQube
- **Dynamic Testing**: OWASP ZAP, Burp Suite
- **Dependency Scanning**: npm audit, Snyk, WhiteSource
- **Container Security**: Trivy, Clair, Twistlock

### Monitoring Solutions
- **SIEM**: Splunk, ELK Stack, QRadar
- **Log Analysis**: Graylog, Fluentd, Logstash
- **Network Monitoring**: Wireshark, Suricata, Zeek
- **Application Monitoring**: New Relic, Datadog, AppDynamics

### Development Security
- **Secure Coding**: OWASP guidelines, secure coding standards
- **Code Review**: Security-focused review checklists
- **Testing**: Security test cases, penetration testing
- **Training**: Security awareness, secure development training

---

## 📚 OWASP Top 10 for LLMs Mapping

| OWASP LLM Risk | MCP Equivalent | Our Coverage |
|----------------|----------------|--------------|
| LLM01: Prompt Injection | Prompt Injection, Indirect Injection | ✅ Sections 1,6 |
| LLM02: Insecure Output Handling | Tool Poisoning | ✅ Section 2 |
| LLM03: Training Data Poisoning | Supply Chain | ✅ Section 10 |
| LLM04: Model Denial of Service | Session Hijacking | ✅ Section 7 + Rate limiting |
| LLM05: Supply Chain Vulnerabilities | Supply Chain | ✅ Section 10 |
| LLM06: Sensitive Information Disclosure | Excessive Permissions, Token Passthrough | ✅ Sections 5,9 |
| LLM07: Insecure Plugin Design | Tool Poisoning, Dynamic Changes | ✅ Sections 2,3 |
| LLM08: Excessive Agency | Excessive Permissions, Confused Deputy | ✅ Sections 5,8 |
| LLM09: Overreliance | Session Hijacking | ⚠️ User education + Section 7 |
| LLM10: Model Theft | Token Passthrough, Auth Issues | ✅ Sections 4,9 |

---

## 🎯 Implementation Priority Matrix

### High Priority (Implement First)
1. **Input Validation** - Foundation of security
2. **Authentication** - Control access to system
3. **Permission Controls** - Limit user capabilities
4. **Code Safety** - Prevent arbitrary execution

### Medium Priority (Implement Next)
1. **Tool Security** - Secure tool ecosystem
2. **External Data Validation** - Secure data sources
3. **Monitoring** - Detect and respond to threats
4. **Configuration Security** - Harden system settings

### Lower Priority (Nice to Have)
1. **Advanced Analytics** - Sophisticated threat detection
2. **Automated Response** - Self-healing capabilities
3. **Integration Security** - Secure third-party connections
4. **Compliance Reporting** - Regulatory requirements

---

## ⚠️ Common Mistakes to Avoid

### Development Mistakes
- Using `eval()` or `new Function()` with user input
- Concatenating user input directly into system prompts
- Implementing authentication without session management
- Granting excessive permissions by default
- Skipping input validation for "trusted" sources

### Deployment Mistakes
- Using default credentials in production
- Exposing debug endpoints or information
- Insufficient logging and monitoring
- Missing security headers and configurations
- Inadequate error handling

### Operational Mistakes
- Irregular security assessments
- Poor incident response planning
- Inadequate staff security training
- Delayed security patch application
- Insufficient backup and recovery testing

---

## 📞 Emergency Response Contacts

### Internal Contacts
- **Security Team**: [Contact Information]
- **System Administrators**: [Contact Information]
- **Development Team**: [Contact Information]
- **Management**: [Contact Information]

### External Contacts
- **Vendor Support**: [Vendor contact information]
- **Security Consultants**: [Consultant contacts]
- **Law Enforcement**: [If required for severe incidents]
- **Legal Counsel**: [Legal team contacts]

---

## 📈 Metrics to Track

### Security Metrics
- Number of security incidents per month
- Mean time to detection (MTTD)
- Mean time to response (MTTR)
- Percentage of systems with current security patches
- Number of failed authentication attempts
- Percentage of users with MFA enabled

### Operational Metrics
- System uptime and availability
- Performance impact of security controls
- User satisfaction with security measures
- Training completion rates
- Compliance audit results

---

This quick reference guide provides immediate access to essential MCP security information. Keep it handy for rapid security assessments and incident response.
