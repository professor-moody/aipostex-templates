## Template Submission

### Template ID
`{paste template id here}`

### Service Family
{e.g., ollama, mcp, jupyter, ray, mlflow, etc.}

### Template Type
- [ ] Detection (auth, enum, disclosure)
- [ ] Exploit (requires `--mode full`)
- [ ] CVE-specific

### Description
{Brief description of what this template detects/exploits and why it matters.}

### Testing
- [ ] Tested against a real instance of the target service
- [ ] Tested against a mocked/lab instance
- [ ] Verified no false positives on non-target services
- [ ] YAML validated (yamllint or equivalent)

### Checklist
- [ ] Template ID follows naming convention (`service-type-NNN-description`)
- [ ] All required fields present (`id`, `info`, `detect`, `checks`)
- [ ] Severity rating is appropriate
- [ ] Finding descriptions are clear and actionable
- [ ] Exploit templates include `mode: exploit`
