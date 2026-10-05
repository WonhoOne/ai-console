# ai-console — historical / legacy repository

This repository historically hosted or planned **AI Voice, Employee Console, and integration-related client work**. It is retained for historical material and existing artifacts; new active feature implementation does not belong here by default. This reconciliation does not delete, rename, archive, or repurpose the repository.

Current active work follows the approved [repository responsibilities in docs/main](https://github.com/WonhoOne/docs/blob/main/architecture/repository-responsibilities.md):

- **AI Voice — 이한결:** [`WonhoOne/frontend`](https://github.com/WonhoOne/frontend), only in `src/integrations/voice/**` and `src/features/voice-bridge/**`.
- **Employee Console — 김태우:** the current Frontend track in `WonhoOne/frontend`, alongside Customer Frontend and Frontend ↔ Backend live integration.

Always read the latest approved baseline and contracts from [`docs/main`](https://github.com/WonhoOne/docs/tree/main) rather than relying on version numbers embedded in this legacy repository. Reconciliation reference: `c5b763253bb4acd8e4e4c6db0a736be8f1c247fc` (Baseline v0.2-era contracts and docs PRs #11/#12).

Older documents, issues, and PRs may reflect historical/superseded assignments. Preserve that history and follow current `docs/main` for ownership and handoff. PR #5's Employee Console work-order context may be superseded and requires separate Frontend-owner handling; this task does not modify, close, merge, or rewrite it.

Voice semantics and public APIs are unchanged: Voice may update `ReservationDraft`, but Reservation creation requires review and explicit GUI submission. Backend remains the final validation authority. See [AGENTS.md](AGENTS.md) for reading requirements and repository scope.
