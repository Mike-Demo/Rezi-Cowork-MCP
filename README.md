# Rezi Cowork MCP Plugin Source

This repository contains an Open Plugin source package that can be imported into a Microsoft Copilot Cowork project and connected to the hosted Rezi MCP server.

## Initial repository assessment

Before these changes, the repository contained only:

```text
README.md
```

That layout did **not** match any supported plugin source format:

- **Claude plugin:** missing `.claude-plugin/plugin.json`
- **Cursor plugin:** missing `.cursor-plugin/plugin.json`
- **Agent Plugins 1.0.0:** missing `plugin.json` and `skills/`
- **Microsoft 365 Agents Toolkit source:** missing importable plugin manifest, MCP config, and skills

For `atk import openplugin`, the missing required source files were:

- `.plugin/plugin.json`
- `.mcp.json`
- `skills/rezi-resume/SKILL.md`

## Source layout

```text
.plugin/
  plugin.json
.mcp.json
skills/
  rezi-resume/
    SKILL.md
README.md
```

## Rezi MCP connection

- **Remote endpoint:** `https://api.rezi.ai/mcp`
- **Transport:** Streamable HTTP
- **Authentication:** Rezi's interactive user-scoped OAuth sign-in flow
- **Core resume tools used by this plugin source:**
  - `list_resumes`
  - `read_resume`
  - `write_resume`

No credentials, tokens, cookies, API keys, session IDs, or client secrets are stored in this repository.

## Skill behavior

The bundled `rezi-resume` skill is designed to:

- list the user's resumes before asking them for a resume ID;
- read the selected resume before proposing edits;
- review resumes and suggest improvements without fabricating facts;
- tailor a resume to a job description while preserving existing factual content;
- require explicit user approval immediately before `write_resume`;
- distinguish clearly between creating a new resume and updating an existing one;
- preserve existing content unless the user requests removal;
- avoid exposing unnecessary personal resume information.

## Install the Microsoft importer

```bash
npm install -g @microsoft/m365agentstoolkit-cli
atk --version
```

## Import into a Microsoft Copilot Cowork project

`atk import openplugin` requires legal URLs in the generated manifest. This repository does not include owner-specific legal pages, so replace the placeholders below with your real URLs.

```bash
atk import openplugin \
  --path . \
  --output ./rezi-cowork-plugin \
  --privacy-url https://YOUR-DOMAIN.example/privacy \
  --terms-url https://YOUR-DOMAIN.example/terms
```

## Manual configuration after import

Rezi uses an interactive OAuth flow with browser sign-in, dynamic client registration, and PKCE. Do **not** hard-code auth metadata, scopes, client secrets, or access tokens into this repository.

After import:

1. Open the generated Microsoft 365 Agents Toolkit project.
2. Review the generated agent connector for the Rezi MCP server.
3. Supply your real privacy and terms URLs in the import command if you used placeholders.
4. Register the generated **placeholder** Microsoft plugin-vault/auth reference ID in your Microsoft environment before use.
5. If automatic auth detection is unavailable in your environment, rerun the import with:

```bash
atk import openplugin \
  --path . \
  --output ./rezi-cowork-plugin \
  --privacy-url https://YOUR-DOMAIN.example/privacy \
  --terms-url https://YOUR-DOMAIN.example/terms \
  --default-auth-type OAuthPluginVault
```

The importer can generate a placeholder auth reference ID, but the real Microsoft-side registration must be completed manually.

## Validation target

This source package is intended to be compatible with `atk import openplugin` and to produce a Cowork-ready project with:

- one remote MCP connector pointing at `https://api.rezi.ai/mcp`;
- one bundled skill at `skills/rezi-resume/SKILL.md`;
- no embedded credentials;
- guidance that treats `write_resume` as a consequential operation.