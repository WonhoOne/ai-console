# AI Voice / Employee Console Agent Instructions

This repository implements **Voice Recognition, Employee Console, and integration-related client work**.

## Mandatory reading before implementation

Before writing or modifying code, read the latest approved documents in `WonhoOne/docs`:

1. `baseline/BASELINE-v0.1.md`
2. `requirements/requirements.md`
3. `requirements/domain-model.md`
4. `requirements/business-rules.md`
5. `architecture/system-architecture.md`
6. `architecture/repository-responsibilities.md`
7. `architecture/voice-contract.md`
8. `api/api-spec-draft.md`
9. `CONTRIBUTING.md`
10. `AGENTS.md`

Docs repository: https://github.com/WonhoOne/docs

Do not begin implementation against an unapproved local assumption when the required baseline is not yet available on the approved docs branch.

## Repository responsibilities

- Speech-to-Text integration / PoC
- Restricted travel command interpretation
- Voice ↔ Customer flow / Backend API connection
- Employee Console for tour and inventory operations
- Integration support and E2E scenarios

## Non-negotiable rules

- Voice scope is limited-command based, not free-form travel consultation unless the docs baseline changes.
- Use approved Backend APIs. Do not bypass Backend services.
- Do not access MySQL directly.
- Do not reimplement Backend business rules as the final authority.
- Do not invent new Voice commands as shared contract without updating the Voice Contract.
- Do not invent endpoint paths or request/response structures.
- Do not resolve TBD items by assumption.
- If a required command or API is missing, surface the gap and propose the docs change first.
- If code and docs conflict, stop and surface the conflict.

## Voice processing principle

```text
Speech
→ Speech-to-Text
→ Command interpretation
→ Customer GUI state update and/or Backend API
→ Backend validation
```

## PR expectations

Every implementation PR should identify:

- related Requirement IDs
- Voice command(s) or Employee flow affected
- Backend API endpoints used
- E2E / integration test evidence
- whether any shared contract changed
