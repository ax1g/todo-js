# Security Policy

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| 1.1.x   | ✅ Yes             |
| 1.0.x   | ⚠️ Limited support |
| < 1.0   | ❌ No              |

## Reporting a Vulnerability

If you discover a security vulnerability in ToodooL, please report it responsibly. **Do not** open a public GitHub issue for security vulnerabilities.

### Reporting Process

1. **Email**: Send a detailed report to the maintainer (check GitHub profile)
2. **Information to Include**:
   - Description of the vulnerability
   - Steps to reproduce
   - Potential impact
   - Suggested fix (if available)
   - Your contact information for follow-up

3. **Response Timeline**:
   - Initial acknowledgment: Within 48 hours
   - Investigation: Within 1 week
   - Patch or public disclosure: As soon as possible after fix is available

4. **Confidentiality**:
   - All vulnerability reports will be kept confidential
   - Reporters will be credited in security advisories (if desired)
   - We will not publicly disclose details until a fix is available

## Security Considerations

### What We Protect

ToodooL is a **client-side only application** with the following security characteristics:

✅ **Secure**:
- Data stored only in browser localStorage (no transmission)
- No server communication required
- No user accounts or authentication needed
- No tracking or telemetry
- No third-party data sharing

⚠️ **Limited Scope**:
- No backend security (client-side only)
- No cloud sync (no data transmission)
- No authentication system
- No API endpoints to compromise

### Known Limitations

1. **localStorage Scope**:
   - Data accessible to any script on the same origin
   - Not encrypted (browser security model)
   - Cleared when browser data is deleted
   - Same-origin policy applies

2. **No Encryption**:
   - Todos stored in plaintext in localStorage
   - Appropriate for local personal data
   - Not suitable for sensitive information

3. **Client-Side Only**:
   - Vulnerability only affects local browser
   - No risk to server (there is none)
   - No risk to other users' data

4. **XSS Considerations**:
   - Using `innerHTML` for rendering (necessary for formatting)
   - Data sanitization through storage layer
   - No user input validation (considered for future versions)

### Browser Security

ToodooL relies on browser security features:
- Same-origin policy
- localStorage isolation per origin
- ContentSecurityPolicy (configurable by website hosting it)
- Modern browser security defaults

## Security Best Practices

### For Users

1. **Keep Browser Updated**: Ensure your browser is current with security patches
2. **Use HTTPS**: Access only via HTTPS (https://theankushgautam.github.io/Toodool/)
3. **Avoid Sensitive Data**: Don't store passwords or financial data in todos
4. **Browser Privacy**: Use private/incognito mode for sensitive browsing
5. **Device Security**: Keep your device secure and unlocked

### For Contributors

1. **Dependency Updates**: Keep all npm dependencies updated
   ```bash
   npm audit
   npm audit fix
   ```

2. **Code Review**: All code contributions reviewed before merge

3. **No Secrets**: Never commit API keys, tokens, or sensitive data

4. **Linting**: Run ESLint to catch code quality issues
   ```bash
   npm run lint:fix
   ```

5. **Testing**: Test all changes thoroughly before PR

## Known Issues

### Security-Related

| Issue | Severity | Status | Details |
|-------|----------|--------|---------|
| No input sanitization | Low | Planned | Will add HTML escaping in v1.2 |
| innerHTML usage | Low | Intended | Required for functionality, isolated scope |
| No HTTPS enforcement | N/A | N/A | Enforced by GitHub Pages |
| localStorage unencrypted | Low | N/A | Expected behavior for local app |

### Mitigations

1. **Input Validation** (Planned v1.2):
   - Maximum length checks
   - HTML escaping for display
   - Prevent XSS in user input

2. **Storage Validation** (Completed v1.1):
   - Type checking
   - Error handling for malformed data
   - Safe JSON parsing

3. **Dependency Scanning** (Ongoing):
   - npm audit regularly
   - Automated security updates
   - Vulnerable package removal

## Compliance

### Standards

- ✅ **OWASP Top 10** (as applicable to client-side app):
  - No injection attacks possible (no backend)
  - No broken authentication (no auth system)
  - Sensitive data protected by browser security
  - No XML/XXE processing
  - No direct access control breaches

- ✅ **Privacy**:
  - No data collection
  - No tracking
  - No third-party sharing
  - All data local only

### GDPR

- ✅ Compliant: No personal data collection or processing
- ✅ No need for data deletion (local browser storage)
- ✅ No privacy policy required (no data transmission)

## Incident Response

### If a Vulnerability is Discovered

1. **Investigate**: Understand scope and impact
2. **Assess**: Determine severity and affected versions
3. **Fix**: Develop and test patch
4. **Release**: Publish security update with advisory
5. **Communicate**: Notify users through security advisory
6. **Patch**: Update all affected versions

### Severity Levels

- **Critical**: Immediate patch release, public advisory
- **High**: Patch release within 1 week, advisory notification
- **Medium**: Patch in next regular release, advisory
- **Low**: Document and include in next version

## Security Updates

Subscribe to updates:
- Watch the GitHub repository for releases
- Check CHANGELOG.md for security notes
- Review security advisories

### Patching Policy

- All versions receive security patches
- Patches released as soon as fixes are verified
- No forced upgrade period

## Third-Party Dependencies

### Audit Trail

All dependencies are regularly audited:

```bash
npm audit
```

**Current Dependencies**:
- `date-fns`: ✅ Regularly updated, no known vulnerabilities
- `webpack`: ✅ Security actively maintained
- `eslint`: ✅ Security focused

**DevDependencies**: Scanned for vulnerable versions

### Removed/Deprecated

- No deprecated dependencies in use
- Old versions phased out as new ones release
- Minimal dependency footprint maintained

## Security Future Roadmap

### v1.2.0 (Planned)
- [ ] Input sanitization for XSS protection
- [ ] Content Security Policy header guidance
- [ ] Security documentation improvements

### v1.3.0 (Planned)
- [ ] Service worker for offline (with security implications)
- [ ] Optional data export encryption
- [ ] Security audit checklist

### v2.0.0 (Planned)
- [ ] Optional backend sync (with authentication)
- [ ] End-to-end encryption option
- [ ] Security token implementation

## Contact

For security concerns or vulnerability reports:

1. **Do NOT** open a public GitHub issue
2. Check the repository for security contact information
3. Email the maintainer directly with `[SECURITY]` in subject line
4. Allow time for investigation and patching before disclosure

## Disclaimer

This security policy applies to ToodooL as a client-side application. Users are responsible for:
- Keeping their devices and browsers secure
- Maintaining strong device security practices
- Using HTTPS when accessing the application
- Understanding localStorage limitations

ToodooL is provided as-is. While we strive for security, we make no guarantees regarding absolute security of local data. Users are responsible for their own data protection.

## References

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [Browser Security Model](https://developer.mozilla.org/en-US/docs/Web/Security)
- [localStorage Security](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)
- [Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)

---

**Last Updated**: February 2024
**Version**: 1.0

Thank you for helping keep ToodooL secure! 🔒
