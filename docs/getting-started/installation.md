# Installation

## CLI Tool

Install the `system-spec` CLI:

```bash
go install github.com/plexusone/systemspec-deploy/cmd/system-spec@latest
```

Verify installation:

```bash
system-spec help
```

## Go SDK

Add to your Go module:

```bash
go get github.com/plexusone/systemspec-deploy@latest
```

Import packages:

```go
import (
    "github.com/plexusone/systemspec-deploy/spec"
    "github.com/plexusone/systemspec-deploy/graph"
    "github.com/plexusone/systemspec-deploy/render"
)
```

## Requirements

- Go 1.24 or later
- For rendering: D2, Mermaid CLI, or GraphViz (optional, for image generation)

## Rendering Dependencies

To render diagrams to images (optional):

=== "D2"

    ```bash
    # macOS
    brew install d2

    # Linux
    curl -fsSL https://d2lang.com/install.sh | sh
    ```

=== "Mermaid"

    ```bash
    npm install -g @mermaid-js/mermaid-cli
    ```

=== "GraphViz"

    ```bash
    # macOS
    brew install graphviz

    # Ubuntu/Debian
    apt-get install graphviz
    ```
