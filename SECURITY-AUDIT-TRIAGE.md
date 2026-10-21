# Security audit triage — 2026-10-21

Parent issue: [#2 — Security audit 2026-10-21](https://github.com/Mike-Demo/Rezi-Cowork-MCP/issues/2)

## Confirmed findings

The audit reports **0 confirmed findings**. There are no remediation issues to prioritize or file; neither of the two leads below is confirmed. No code changes are proposed.

## Ready-to-file issue

**Title**

`[security][needs-validation] Audit leads to validate`

**Body**

```markdown
Parent issue: #2

Validate each deployment-dependent audit lead below. These are leads, not confirmed findings; no severity is assigned. Record the missing fact for each item before deciding whether remediation is warranted.

- [ ] **Remote MCP connection has no tool allowlist or server-identity pinning; agent inherits whatever tools/descriptions api.rezi.ai presents**
  - Fingerprint: `rezi-mcp-no-tool-allowlist-or-server-pinning`
  - Claimed root cause: .mcp.json defines only type, transport, url and description for the rezi server, and .plugin/plugin.json adds no allowlist, pin, or schema constraint, so nothing in the package deterministically constrains which server-presented tools the agent inherits.
  - Missing fact: Two linked deployment facts not visible in source: (1) the live tool list (names, descriptions, schemas) actually served at https://api.rezi.ai/mcp for an authenticated user session, and (2) whether the Microsoft Copilot Cowork host restricts the imported rezi connector to the three README-named tools (list_resumes, read_resume, write_resume) and …

- [ ] **Consequential write_resume is gated only by prompt text, with no deterministic approval binding in the plugin package**
  - Fingerprint: `rezi-write-resume-prompt-only-approval`
  - Claimed root cause: The only in-package control for write_resume is prompt text in skills/rezi-resume/SKILL.md; there is no code, tool gate, argument binding, or host configuration in the package that deterministically binds execution to the exact change set the user approved.
  - Missing fact: Whether Microsoft Copilot Cowork imposes a deterministic per-call confirmation (bound to tool name, resume_id, and the full argument object, not reusable across retries or changed arguments) for write_resume and other OAuthPluginVault MCP write tools imported via atk import openplugin. If Cowork enforces such a confirmation, the claim is defeated;…
```
