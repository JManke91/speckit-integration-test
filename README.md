# speckit-integration-test

Reference implementation repository for the **SDLC-Orchestrator → llm-wiki → SpecKit** delivery
chain (integration plan A7): a coding agent in this repo pulls its SpecKit artifacts
(constitution, feature spec, plan context) from the wiki's MCP server — no wiki clone, no
SharePoint access.

## How it connects

`.mcp.json` starts the published server `@dswinscoe/sdlc-speckit-mcp` (GitHub Packages) via
`npx`. The server reads the CI-published exports bundle from Azure Blob Storage over a single
read-SAS URL. Only reviewed, merged wiki state is ever served.

## One-time developer setup

1. **GitHub Packages auth** (the package lives in a private repo) — `~/.npmrc`:

   ```
   @dswinscoe:registry=https://npm.pkg.github.com
   //npm.pkg.github.com/:_authToken=<token with read:packages>
   ```

2. **Exports read-SAS URL** (from the team vault) — export it in your shell profile:

   ```bash
   export SPECKIT_EXPORTS_SAS_URL="https://sdlcasyncstorage.blob.core.windows.net/wiki-speckit-exports-stage/speckit-exports.json?<read-sas>"
   ```

   `.mcp.json` expands `${SPECKIT_EXPORTS_SAS_URL}` at startup, so the secret never lands in git
   and SAS rotation needs no commit.

3. Start your coding agent (e.g. Claude Code) in this repo. The `sdlc-speckit` server exposes
   `list_projects`, `list_features`, `get_constitution`, `get_feature_spec`, `get_plan_context`
   and `get_speckit_bundle` (with placement instructions).

## Bootstrap a feature

Ask the agent to call `get_speckit_bundle` — it places:

| Artifact | Target |
|---|---|
| Constitution | `.specify/memory/constitution.md` |
| Feature spec | `specs/<feature>/spec.md` |
| Plan context | pasted into the `/speckit.plan` prompt |

`SDLC_PROJECT_NAME` in `.mcp.json` pins the default project (`lisa_test` for the current
showcase); pass `projectName` on any tool call to override, or remove the env to choose
per call. New projects appear automatically once their wiki compile is merged — nothing to
change here.
