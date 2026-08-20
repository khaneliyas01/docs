# Digital Brain

A personal data and knowledge system — a unified, secure, searchable interface to everything you digitally own.

> **Vision:** If you own it or have authorized access to it, the Digital Brain should have a secure and documented way to discover, retrieve, understand, connect, and use that data.

---

## What it is

The Digital Brain is a **personal data operating system**. It provides a controlled data access layer over your digital ecosystem — email, notes, files, photos, finances, calendar, Git, Docker, databases — without blindly copying everything into one database.

It indexes, caches, synchronizes, transforms, or stores locally **only where appropriate**, always with a documented access method and a clear link back to the original source.

## Who it's for

One user. Private by default. No sharing, no collaboration, no multi-user mode, no public access in MVP.

## Core principles

| Principle | What it means |
| :--- | :--- |
| **User owns the data** | The system never assumes a service provider's API is your only representation of your information. API access, export formats, local files, databases, webhooks, sync, backups, archives — all supported where practical. |
| **Local-first where practical** | Sensitive data stays local unless you explicitly permit an external transfer, it's necessary for the operation, the destination is trusted, and the transfer is secured. |
| **Access, don't assume** | Every data source has an explicit access method — filesystem, API, database connector, S3-compatible API, and so on. No magic, no hidden copies. |
| **Traceable answers** | When the system answers a question, you can see which sources contributed, what was retrieved, and where the original lives. |

## MVP scope (Phase 1)

**Sources:**

| Source | Access method |
| :--- | :--- |
| Email | IMAP / SMTP |
| Notes | File / folder-based, markdown / text |
| Financial details | File import (CSV / PDF) or manual entry |

**What the MVP does with them:**

- Discover and retrieve from the configured sources
- Index and cache selected data locally
- Traceable search across sources
- Source health as a status strip (not a dashboard)

**What the MVP explicitly does NOT do:**

- Multi-user or sharing / collaboration
- Public access by default
- External LLM calls in MVP
- Admin roles
- A specific retention policy or compliance/regulatory posture

## Phase 2 (planned)

- Photos — via Immich MCP
- Files — via Nextcloud MCP

## Design direction

These decisions came out of calibration with `@artist`:

- **Search-first interface** — a big query bar, not a chat window
- **No agent persona** — agents are background mechanics, not characters you talk to
- **Source health as a status strip**, not a dashboard
- **Dark mode baseline** — system-following default, with manual override
- **Calm, minimal, personal feel** — Notion-level polish, quieter
- **No chat UI, no conversation history, no autonomous agent behavior**

## Architecture (summary)

```
User Query
    ↓
Search / Retrieval Layer
    ↓
Source Access Layer  ←  one explicit access method per source
    ↓
Data Sources
    ├── Email        (IMAP/SMTP)
    ├── Notes        (files / folders, markdown / text)
    ├── Financials   (CSV / PDF import, manual entry)
    ├── Photos       (Immich MCP — Phase 2)
    ├── Files        (Nextcloud MCP — Phase 2)
    ├── Calendar
    ├── Git / GitHub
    ├── Docker
    ├── Databases
    └── Cloud Storage (provider API / S3-compatible)
```

- **Network:** LAN access plus WireGuard for remote; no public exposure by default.
- **Security:** per-source credential handling, AES-256 at rest, immutable audit log.

## Tech stack

Some choices are still being confirmed with engineering — the sections below reflect the current plan, not a final decision.

| Layer | Current plan | Status |
| :--- | :--- | :--- |
| Backend | To be confirmed with `@coder` / `@lead` | Under discussion |
| Frontend | To be confirmed with `@coder` / `@lead` | Under discussion |
| Data layer | Indexed, cached, synchronized, transformed, or stored locally as appropriate | Under discussion |
| Hosting | Local / private for personal use | Planned |

If you need firm answers on backend framework, frontend framework, or DB choice, `@pm` can pull them from `@coder` or `@lead`, or you can take them from the planning docs in the project worktree.

## Status

- **Planning:** Done — see `IDEA.md` in the project worktree.
- **Phase 1 sources:** Email, Notes, Financials — scoped.
- **Phase 2 sources:** Photos (Immich MCP), Files (Nextcloud MCP) — planned.
- **Live deployment:** None yet.
- **Repo:** Not yet created / not yet public.

## Open questions

- Final backend and frontend framework choices (with `@coder` / `@lead`)
- DB choice
- Local storage and caching strategy details
- Phase 2 integration specifics for Immich MCP and Nextcloud MCP

---

*Documentation page for the Digital Brain project. Maintained by the `docs` agent. No secrets in this file.*
