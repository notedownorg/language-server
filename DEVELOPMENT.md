# Notedown Language Server Development Guide

Development guide for the Notedown Language Server Protocol (LSP) implementation.

## Prerequisites

**Option 1: Nix (Recommended)**
- Install [Nix](https://github.com/DeterminateSystems/nix-installer)
- All dependencies provided automatically via `flake.nix`

**Option 2: Manual Setup**
- Go 1.24.4+
- golangci-lint
- licenser

## Quick Start

```bash
# Clone and build
git clone https://github.com/notedownorg/language-server.git
cd language-server
make install

# Verify
notedown-language-server --version
```

## Development Workflow

### Build and Install

```bash
make install    # Build and install to $GOPATH/bin
```

### Testing

```bash
make test       # Run all tests
make check      # Full check: clean, hygiene, lint, test
```

### Code Quality

```bash
make hygiene    # Format and tidy
make lint       # Run linter
make all        # Hygiene + test + dirty check
```

## Testing with Neovim

Configure the Neovim plugin to use your local build:

**Lazy.nvim:**
```lua
{
    "notedownorg/notedown.nvim",
    config = function()
        vim.treesitter.language.register('markdown', 'notedown')
        require("notedown").setup({
            server = {
                cmd = { vim.fn.expand("$GOPATH") .. "/bin/notedown-language-server", "serve" }
            }
        })
    end,
}
```

**Packer.nvim:**
```lua
use {
    "notedownorg/notedown.nvim",
    config = function()
        vim.treesitter.language.register('markdown', 'notedown')
        require("notedown").setup({
            server = {
                cmd = { vim.fn.expand("$GOPATH") .. "/bin/notedown-language-server", "serve" }
            }
        })
    end,
}
```

After changes, run `make install` and restart Neovim.

## Make Targets

| Target | Description |
|--------|-------------|
| `make install` | Build and install to $GOPATH/bin |
| `make clean` | Remove installed binary |
| `make test` | Run all tests |
| `make check` | Clean + hygiene + lint + test |
| `make hygiene` | Format and tidy modules |
| `make format` | Format code and apply licenses |
| `make mod` | Tidy Go modules |
| `make lint` | Run golangci-lint |
| `make dirty` | Check for uncommitted changes |
| `make all` | Hygiene + test + dirty |

## Architecture

```
├── cmd/               # CLI commands
│   ├── root.go
│   ├── serve.go       # LSP server
│   └── version.go
├── pkg/
│   ├── codeexec/      # Code execution
│   ├── constants/     # Constants
│   ├── jsonrpc/       # JSON-RPC protocol
│   ├── lsp/           # LSP types and routing
│   ├── notedownls/    # Notedown LSP handlers
│   │   └── indexes/   # Document indexing
│   └── version/       # Version info
└── main.go
```

### External Dependencies

Imports from `github.com/notedownorg/notedown`:
- `pkg/parser` - Markdown parser with Notedown extensions
- `pkg/config` - Configuration loading
- `pkg/log` - Logging

Configure via `go.mod` replace directive to local repo.

## Debugging

### Enable LSP Logging

```lua
require("notedown").setup({
    server = {
        cmd = {
            vim.fn.expand("$GOPATH") .. "/bin/notedown-language-server",
            "serve",
            "--log-level", "debug",
            "--log-file", "/tmp/notedown-lsp.log"
        }
    }
})
```

Monitor logs:
```bash
tail -f /tmp/notedown-lsp.log
```

### Run Specific Tests

```bash
go test -v ./pkg/notedownls -run TestCompletions
go test -v ./...
```
