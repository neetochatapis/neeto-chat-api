# LLM Guidelines for NeetoChat Docs

## Important Rules

1. **Never make direct changes to the `bundled` folder.** The `bundled` folder is auto-generated and should not be edited manually.

2. **Always make OpenAPI changes in the `docs` folder.** The API reference is generated from the OpenAPI sources in `docs`; that is where those edits belong. Prose pages live outside it, in their own top-level directories (`getting-started`, `cli`, `cli-reference`, `mcp`).

3. **After making changes, run `yarn build`.** That bundles OpenAPI into `bundled/` and regenerates CLI snippets from `cli/catalog.json`. `yarn build:dev` only watches YAML and does not run `cli:build`.

4. **Generated output must be committed with its source.** After OpenAPI edits, check in both `docs/` and `bundled/`. After CLI catalog edits, check in `snippets/cli/**` and `cli-reference/overview.mdx`.

## CLI Documentation Rules

1. **Never edit `snippets/cli/**` or `cli-reference/overview.mdx` by hand.** They are auto-generated from `cli/catalog.json` by `scripts/generate-cli-reference.mjs`. Hand edits are overwritten on the next build.

2. **Refresh `cli/catalog.json` from the CLI, not by hand.** Run `yarn cli:catalog` (with the `neetochat` binary on your `PATH`) to snapshot `neetochat commands`, then `yarn cli:build` to regenerate the flag-table snippets and the commands overview. Build the binary from the latest `main` of the CLI repo so newly added commands are not silently dropped.

3. **Hand-written CLI content lives in `cli/**` (guides) and `cli-reference/*.mdx` (per-resource pages).** The reference pages import the generated flag tables from `snippets/cli/**`; add human-readable headings, usage examples, sample output, and tips there.

4. **New top-level command groups need a page mapping.** Add them to `groupToPage` in `scripts/generate-cli-reference.mjs` (or to `utilityGroups` for CLI-only commands) and to the `Commands` group in `docs.json`, otherwise the generator warns, links them to the utility page, and exits non-zero.

## MCP Documentation Rules

1. **Edit `mcp/**` by hand.** There is no generator and no `tools.json`. Do not invent tool names, config keys, or error codes beyond what `mcp/tools.mdx` already lists.

2. **Fact-check `mcp/**` against the [NeetoChat MCP help article](https://help.neetochat.com/articles/mcp).** The server endpoint, per-client config file paths, and JSON keys come from that article.
