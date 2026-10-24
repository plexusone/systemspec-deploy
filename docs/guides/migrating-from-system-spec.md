# Migrating from system-spec

From v0.2.0, the Go module is published as `github.com/plexusone/systemspec-deploy`. Earlier releases used `github.com/plexusone/system-spec`. This guide covers what to change.

## What changes

| Area | Change needed |
|------|---------------|
| Go imports | Yes: replace the module path |
| `go.mod` | Yes: require the new module |
| CLI install command | Yes: new install path |
| CLI name and subcommands | No: still `system-spec` |
| System spec documents (`system.json`) | No |
| Graphize provider name and graph IDs | No |
| JSON Schema structure | No |
| JSON Schema `$id` | Yes, if your tools match on it |

## Go SDK

### 1. Replace the import paths

Replace `github.com/plexusone/system-spec` with `github.com/plexusone/systemspec-deploy` in your Go files:

```bash
grep -rl 'github.com/plexusone/system-spec' --include='*.go' . \
  | xargs sed -i.bak 's#github.com/plexusone/system-spec#github.com/plexusone/systemspec-deploy#g'
find . -name '*.go.bak' -delete
```

On Linux, `sed -i` works without the `.bak` suffix.

### 2. Update `go.mod`

```bash
go get github.com/plexusone/systemspec-deploy@v0.2.0
go mod tidy
```

`go mod tidy` removes the old `github.com/plexusone/system-spec` requirement once nothing imports it.

### 3. Build and test

```bash
go build ./...
go test ./...
```

Package names, types and functions are unchanged, so no other code changes are needed.

## CLI

Install the CLI from the new path:

```bash
go install github.com/plexusone/systemspec-deploy/cmd/system-spec@latest
```

The binary is still named `system-spec`, and every command works as before:

```bash
system-spec validate system.json
system-spec render system.json --format d2
```

If you install the CLI in CI or in scripts, update the install path there too.

## Graphize

Update the blank import that registers the provider:

```go
import _ "github.com/plexusone/systemspec-deploy/graphize"
```

The provider keeps the name `system-spec`, and nodes keep their IDs (`svc:` and the resource prefixes). Graphs built with v0.1.0 don't need to be rebuilt.

## JSON Schema

The schema printed by `system-spec schema` now has this `$id`:

```
https://github.com/plexusone/systemspec-deploy/schema/system
```

Its structure is the same as in v0.1.0. Update the `$id` only where a tool, editor setting or schema registry looks the schema up by that value.

## Staying on v0.1.0

v0.1.0 remains available at the old module path, so existing builds keep working:

```
require github.com/plexusone/system-spec v0.1.0
```

New releases are published only under `github.com/plexusone/systemspec-deploy`.
