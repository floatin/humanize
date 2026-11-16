# Domain Docs

How the engineering skills should consume this repo's domain documentation when exploring the codebase.

## Before exploring, read these

- **`GLOSSARY.md`** at the repo root, or
- **`GLOSSARY-MAP.md`** at the repo root if it exists: it points at one `GLOSSARY.md` per context. Read each one relevant to the topic.
- **`docs/adr/`**: read ADRs that touch the area you're about to work in.

If any of these files don't exist, **proceed silently**. Don't flag their absence; don't suggest creating them upfront. The `/domain-modeling` skill (reached via `/grill-with-docs` and `/improve-codebase-architecture`) creates them lazily when terms or decisions actually get resolved.

## This repo's domain language is already normative

The domain is specified in **`protocol/`** — a set of normative documents with tags, each paragraph numbered. Read it before proposing anything that touches behaviour.

| Document | Tag | States |
| --- | --- | --- |
| `protocol/core.md` | `[CORE]` | The conventions every document follows, the shared terms, and the behaviour common to every interface: epics, agents, accounts, placements, environments, chats, side questions, monitoring, backups, telemetry, updates |
| `protocol/python.md` | `[PYTHON]` | The flow API (`hmz.flows`) and the SDK (`hmz.sdk`) |
| `protocol/cli.md` | `[CLI]` | The command line: invocation, output, exit statuses, `hmz exec`, `hmz web`, `hmz daemon`, `hmz relay` |
| `protocol/daemon.md` | `[DAEMON]` | The daemon |
| `protocol/web.md` | `[WEB]` | The web interface |
| `protocol/tui.md` | `[TUI]` | The terminal interface |
| `protocol/desktop.md` | `[DESKTOP]` | The desktop application |
| `protocol/files.md` | `[FILES]` | The settings file, the environment variables, and every file and directory hmz creates, reads, writes or removes |
| `protocol/wire.md` | `[WIRE]` | Carriers, sessions and RPC: mutual TLS, pairing, WebRTC, the relay, the frames, and the JSON-RPC 2.0 session whose methods `protocol/openrpc.json` lists |
| `protocol/openrpc.json` | `[OPENRPC]` | The JSON-RPC method list |

`AGENTS.md` says the protocol MUST be adhered to and MUST NOT be modified unless explicitly instructed. So when a protocol paragraph states a rule, that is the rule: read it, follow it, and surface a contradiction with the code rather than resolving it silently.

`protocol/core.md` §2.2.5–2.2.9 also fix the shape of the rules themselves: every normative paragraph is numbered and cited as `Section 4.6.2.1`; where the implementation and the protocol disagree, **the protocol states the intended behaviour**; and the protocol **never calls an epic a run**.

## The words to use

Until a `GLOSSARY.md` exists, take the domain words from `protocol/core.md` §2 and use them exactly:

- **hmz**, **user**, **machine**, **this machine**, **home**, **workspace**
- **flow**, **flowverse**, **epic**, **role**, **turn**, **side question** (also **btw**)
- **harness**, **agent**, **session**, **provider** (also **account**), **harness machine**

Plus the concepts the core document has a whole section for: **placement**, **environment**, **chat**, **monitoring**, **backup**, **telemetry**, **update**.

`docs/reference/` and `docs/features/` carry the user-facing view of the same things; `docs/user/` the guides.

Note the Rust crate at `crates/coganchor` has its own tests (`cargo test`), separate from the Python test layout `AGENTS.md` describes.

## File structure

Single-context repo (this one):

```
/
├── GLOSSARY.md
├── docs/adr/
│   ├── 0001-a-hard-to-reverse-decision.md
│   └── 0002-another-hard-to-reverse-decision.md
└── src/
```

## Use the glossary's vocabulary

When your output names a domain concept (in an issue title, a refactor proposal, a hypothesis, a test name), use the term as defined in `GLOSSARY.md`. Don't drift to synonyms the glossary explicitly avoids.

If the concept you need isn't in the glossary yet, that's a signal: either you're inventing language the project doesn't use (reconsider) or there's a real gap (note it for `/domain-modeling`).

## Flag ADR conflicts

If your output contradicts an existing ADR, surface it explicitly rather than silently overriding:

> _Contradicts ADR-0007 (event-sourced orders), but worth reopening because…_
