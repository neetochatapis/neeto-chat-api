# NeetoChat API Docs

This repository contains the documentation for the [NeetoChat APIs](https://apidocs.neetochat.com/getting-started/introduction), built using [Mintlify](https://mintlify.com/).

## Development Setup

1. ### Install Mintlify CLI globally

   ```bash
   npm i -g mint
   ```

2. ### Install project dependencies

   This project uses Yarn 4 through Corepack. Enable Corepack once per machine, then install:

   ```bash
   corepack enable
   yarn install
   ```

3. ### Make code changes in docs folder

4. ### Preview the changes

   ```bash
   yarn docs:preview
   ```

   A local preview will be available at `http://localhost:3000`. You can customize the port using the `--port` flag:

   ```bash
   yarn docs:preview --port 3333
   ```

   DO NOT MAKE CODE CHANGES IN BUNDLED FOLDER.

5. ### Build the API

   After OpenAPI changes run `yarn build` (or `yarn build:dev` to watch YAML).
   That updates `bundled/`, which is what Mintlify uses. `yarn build` also
   regenerates CLI snippets. You should NEVER make changes to the `bundled`
   folder directly.

   Refer to [llm.md](llm.md) for more info.

## CLI command reference

The pages under `cli-reference/` are part generated. `cli-reference/overview.mdx` and
everything under `snippets/cli/` are written by `scripts/generate-cli-reference.mjs` from
`cli/catalog.json`, which is a snapshot of the CLI's own command catalog. Never hand-edit
those files; the next build overwrites them.

The per-resource pages next to the overview (`cli-reference/team-members.mdx` and friends) are
hand-written and import the generated flag tables.

To refresh after the CLI gains a command or a flag, with a `neetochat` binary built from
the CLI repo's latest `main` on your `PATH`:

```bash
yarn cli:catalog   # re-snapshot cli/catalog.json from the binary
yarn cli:build     # regenerate the snippets and the overview
```

`yarn build` runs `cli:build` but not `cli:catalog`, so CI never needs the binary and the
docs are never pinned to whatever version a builder happens to have installed.

Every top-level command group needs an entry in `groupToPage` (or in `utilityGroups`) in
`scripts/generate-cli-reference.mjs`. An unmapped group falls back to the utility page,
which will not describe it, so the generator prints a warning and exits non-zero. That
fails `yarn build`, and with it CI, rather than silently filing a new resource under
utility.
