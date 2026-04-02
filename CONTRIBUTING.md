# Contributing to aipostex-templates

Thank you for contributing templates to aipostex! This document covers the conventions, testing checklist, and submission process.

## Template ID Conventions

Template IDs follow a strict naming convention:

```
{service}-{type}-{NNN}-{description}
```

- **service**: Target service family (e.g., `ollama`, `mcp`, `jupyter`, `openai`, `ray`, `mlflow`)
- **type**: Template category:
  - `auth` — Authentication/authorization checks
  - `enum` — Enumeration and metadata disclosure
  - `exploit` — Active exploitation proofs (require `--mode full`)
  - `cve` — Specific CVE checks (use format `cve-YYYY-NNNNN`)
- **NNN**: Three-digit sequence number within the category (e.g., `001`, `002`)
- **description**: Kebab-case brief description

Examples:
- `ollama-auth-001-unauthenticated-api`
- `mcp-exploit-003-schema-poisoning`
- `cve-2025-51471-ollama-token-exposure`

## Required Fields

Every template must have:

```yaml
id: unique-template-id          # Must be globally unique
info:
  name: "Human-readable name"   # Concise, descriptive
  severity: high                 # critical, high, medium, low, info
  author: your-handle            # Your GitHub handle or name
  description: |                 # What this detects and why it matters
    Multi-line description.
  tags: [service, category]      # At least service family + category

detect:                          # How to identify the target service
  - method: GET
    path: /endpoint
    matchers:
      - type: body_contains
        value: "fingerprint"

checks:                          # One or more security checks
  - name: "Check description"
    # ... (see format below)
```

## Check Structure

Each check probes a specific security condition:

```yaml
checks:
  - name: "Human-readable check name"
    method: GET|POST|PUT|DELETE
    path: /api/endpoint
    headers:                     # Optional
      Content-Type: application/json
    body: '{"key": "value"}'     # Optional, for POST/PUT
    matchers:
      - type: status             # HTTP status code
        value: "200"
      - type: body_contains      # Response body substring
        value: "expected"
      - type: header_contains    # Response header substring
        name: "Content-Type"
        value: "application/json"
    extractors:                  # Optional — pull values for findings
      - type: json
        name: variable_name
        path: "json.path.expression"
    severity: high               # Override info-level severity if different
    proof_stage: proof           # discovery, correlation, proof, takeover
    proof_strength: confirmed    # reachable, read-confirmed, confirmed, execution-confirmed
    finding:
      title: "Title with {{variable_name}}"
      description: "Impact description."
      remediation: "How to fix this."
      evidence: |
        Request:  GET /api/endpoint
        Result:   200 OK — {{variable_name}} returned
```

## Exploit Templates

Templates that perform active exploitation or mutating actions:

1. Must include `mode: exploit` in the template metadata
2. Will only run when the user passes `--mode full`
3. Should use bounded proofs — demonstrate impact without causing damage
4. Must document the proof approach in the description

## Testing Checklist

Before submitting a PR, verify:

- [ ] Template ID follows the naming convention
- [ ] All required fields are present
- [ ] `detect` block correctly fingerprints the target service
- [ ] Each check has at least one matcher
- [ ] Finding titles and descriptions are clear and actionable
- [ ] Tested against a real or mocked instance of the service
- [ ] No false positives on common non-target services
- [ ] YAML is valid (run `yamllint` or equivalent)
- [ ] Severity ratings are appropriate for the finding

## Submission Process

1. **Fork** this repository
2. **Create a branch** named `template/{service}-{description}` (e.g., `template/wandb-api-exposure`)
3. **Add your template** to the correct service directory
4. **Test** against a real or mocked instance
5. **Open a PR** using the pull request template
6. A maintainer will review the template for correctness, safety, and style

## Questions?

Open an issue on this repository or the [main aipostex repo](https://github.com/professor-moody/aipostex/issues).
