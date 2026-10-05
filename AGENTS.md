# ai-console Legacy Repository Agent Instructions

`WonhoOne/ai-console` is a **historical / legacy repository**, retained for historical material and existing artifacts. It originally hosted or planned combined AI Voice, Employee Console, and integration-related client work. That assignment is historical; this repository is no longer the default target for new active feature implementation.

Preserve historical files and submitted proposal records. Do not delete, rename, archive, or repurpose the repository in this guidance reconciliation.

## Approved shared source of truth

[`WonhoOne/docs/main`](https://github.com/WonhoOne/docs/tree/main) is the approved shared SSOT. Always read the latest approved baseline and contracts from `docs/main` rather than relying on version numbers embedded in this legacy repository. Feature branches and unmerged PRs remain proposals.

Reconciliation reference: `docs/main@c5b763253bb4acd8e4e4c6db0a736be8f1c247fc`, including Baseline v0.2-era contracts and the responsibility/location reconciliation merged in docs PRs #11 and #12. This reference is not a permanent baseline pin. Earlier baselines are historical records, not pending implementation gates.

Before repository work, read the latest approved Baseline and the mandatory reading list in `docs/main/AGENTS.md`, including:

1. `architecture/repository-responsibilities.md`
2. `architecture/voice-contract.md`
3. `architecture/system-architecture.md`
4. `requirements/requirements.md`
5. `requirements/product-catalog.md`
6. `requirements/domain-model.md`
7. `requirements/business-rules.md`
8. `requirements/non-functional-requirements.md`
9. `api/api-spec-draft.md`
10. `database/erd-draft.md`
11. `CONTRIBUTING.md`
12. `AGENTS.md`

## Current implementation ownership and locations

- **이한결:** Backend, Database, Shared docs management, AWS / SOLAPI runtime, and AI Voice.
- **김태우:** Customer Frontend, Employee Console, and Frontend ↔ Backend live integration.
- **주원호:** no current primary implementation scope.

Active AI Voice implementation lives in `WonhoOne/frontend`, limited to:

- `src/integrations/voice/**` — browser/STT runtime, normalization, interpretation/parser, matcher, and canonical `VoiceCommand` implementation.
- `src/features/voice-bridge/**` — canonical commands connected to existing Frontend Feature actions/queries/mutations and `ReservationDraft` state, coordinated with 김태우.

Employee Console belongs to **김태우's current Frontend track in `WonhoOne/frontend`**. General Frontend ownership remains 김태우; the Voice assignment does not grant write access outside those two directories. Follow current `docs/main` for ownership and handoff.

## Historical assignments and open work

Older documents, issues, and PRs may reflect superseded combined Voice + Employee Console assignments. Preserve them as history; do not treat them as current implementation authorization. The newest ownership comments and approved `docs/main` govern current work. If a newer decision conflicts with approved guidance, stop and surface the conflict.

PR #5's Employee Console work-order context is historical/superseded unless separately handled by the Frontend owner. Do not use it as Voice implementation evidence. This reconciliation does not modify, close, merge, or rewrite PR #5 or its history.

## Cross-repository access and scope

- Work here is historical-material/artifact maintenance or explicitly scoped guidance work. Do not implement new Frontend, Voice, Employee Console, or Backend features here by default.
- `WonhoOne/docs`, `WonhoOne/frontend`, and `WonhoOne/backend` may be inspected read-only for contracts, integration analysis, and handoff context.
- This guidance task permits writes only to this repository's `AGENTS.md` and `README.md`; it does not authorize implementation code or changes in other repositories.
- If active implementation or another repository needs changes, hand them off to its current Owner with the required behavior and contract impact. Cross-repository writes require explicit Owner/team delegation for that scope; follow `docs/main/architecture/repository-responsibilities.md`.
- Shared contract changes require a docs proposal, impact review, and approval before implementation. Do not invent requirements, commands, endpoint paths, or DTOs, or resolve TBD items by assumption.

## Preserved Voice and Backend boundaries

The approved semantics remain unchanged (FR-12, FR-13, BR-12, BR-26, BR-28):

```text
Speech
→ STT
→ command interpretation
→ canonical VoiceCommand
→ Frontend voice bridge
→ existing Feature action/query/mutation boundary
→ Backend validation where applicable
```

- Voice may create or mutate `ReservationDraft`. Voice MUST NOT automatically submit/create a Reservation or perform automatic Reservation POST. After review, the user explicitly submits through the GUI.
- Voice remains limited to approved commands, with GUI fallback. It does not perform authentication credential entry, cancellation/refund/payment, free-form consultation, or Customer Voice mutation of Employee data.
- Use existing Frontend Feature boundaries and approved Backend APIs. Do not access MySQL directly, bypass Backend services, or reimplement Backend business rules as final authority.
- Backend retains final validation authority. If code and approved docs conflict, stop and surface the conflict.

## PR expectations for guidance maintenance

Use a scoped branch and PR. State related requirements or the responsibility gate, approved SSOT reference, files changed, affected contracts, and validation evidence. For this reconciliation, repository responsibility decisions, Voice semantics, public API, and business/domain rules are unchanged. Review the diff, run `git diff --check`, and verify only `AGENTS.md` and `README.md` changed. Do not merge as part of this task.
