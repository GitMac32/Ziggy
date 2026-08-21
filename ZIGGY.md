# Ziggy

GitMac32's build of the Grok Build TUI. **Command: `ziggy`.** This is not an
upstream xAI pull request; it lives on [GitMac32/Ziggy](https://github.com/GitMac32/Ziggy).

## Clone and build (another machine)

```bash
git clone https://github.com/GitMac32/Ziggy.git
cd Ziggy
cargo build -p xai-grok-pager-bin --release
install -m 755 target/release/ziggy ~/.local/bin/ziggy
ziggy --version
```

Needs a Rust toolchain **and protobuf**. The build script runs `bin/protoc`, which is a
[dotslash](https://dotslash-cli.com/) wrapper (not `/usr/bin/cargo`). On a fresh box:

```bash
# cargo lives here, not /usr/bin/cargo
source "$HOME/.cargo/env"

# repo's bin/protoc shebang is `#!/usr/bin/env dotslash`
cargo install dotslash
# alternatively: sudo apt install protobuf-compiler
# (or any `protoc` on PATH — this machine used linuxbrew)

cargo build -p xai-grok-pager-bin --release
```

First release build can take 15–35 minutes. If you see
`/usr/bin/env: ‘dotslash’: No such file or directory` or
`` `protoc` not found ``, install `dotslash` (or `protoc`) and retry.

## Run

```bash
ziggy -m laguna-xs          # Poolside, tools
ziggy -m leanstral          # Mistral
ziggy -m local-granite      # local Ollama, if you configured it
```

Config is still `~/.grok/config.toml` (same as Grok Build). Copy that file
(and do not commit API keys) onto the other machine, or recreate the
`[model_providers.*]` / `[model.*]` blocks there. Without it, `ziggy` behaves
like stock Grok Build and talks to grok.com if you log in.

`main` includes multi-provider custom models and fail-closed credentials so a
custom `base_url` does not inherit the xAI session JWT.
