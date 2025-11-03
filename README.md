# Notedown Language Server

Language Server Protocol (LSP) implementation for [Notedown](https://github.com/notedownorg/notedown) - a modern, extensible markdown format.

## Features

- **Autocompletion**
  - Wikilink completions with `[[` trigger
  - Tag completions with `#` trigger
  - Context-aware suggestions

- **Diagnostics**
  - Broken wikilink detection
  - Configurable diagnostic levels

- **Code Actions**
  - Quick fixes for common issues
  - Refactoring support

- **Navigation**
  - Go to definition for wikilinks
  - Document symbol navigation

- **Code Execution**
  - Execute code blocks in documents
  - Currently supports Go

- **Document Features**
  - Folding ranges
  - Document symbols
  - Workspace management

## Installation

### From Source

```bash
git clone https://github.com/notedownorg/language-server.git
cd language-server
make install
```

This installs the binary to `$GOPATH/bin/notedown-language-server`.

### Binary Releases

Download pre-built binaries from the [releases page](https://github.com/notedownorg/language-server/releases).

## Editor Integration

### Neovim

See the official [notedown.nvim](https://github.com/notedownorg/notedown.nvim) plugin.

### Other Editors

Any LSP-compatible editor can use this language server. Configure your editor to:
- Launch: `notedown-language-server serve`
- File types: `.notedown`, `.nd`

## Configuration

The language server uses Notedown configuration files (`.notedown/settings.yaml`) for workspace settings. See the [Notedown configuration docs](https://github.com/notedownorg/notedown) for details.

## Development

See [DEVELOPMENT.md](DEVELOPMENT.md) for development instructions.

## Architecture

The language server is built on:
- **JSON-RPC** layer for protocol handling
- **LSP** layer for message routing
- **Notedown-specific** handlers for features

Dependencies on the main [Notedown](https://github.com/notedownorg/notedown) repository:
- Parser for markdown with Notedown extensions
- Configuration loading
- Logging utilities

## License

Apache License 2.0 - see [LICENSE](LICENSE) for details.

## Related Projects

- [notedown](https://github.com/notedownorg/notedown) - Main Notedown implementation
- [notedown.nvim](https://github.com/notedownorg/notedown.nvim) - Neovim plugin
