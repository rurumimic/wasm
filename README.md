# wasm

## Docker

- [Introducing the Docker+Wasm Technical Preview](https://www.docker.com/blog/docker-wasm-technical-preview/)
- [Docker + Wasm](https://docs.docker.com/desktop/wasm/) (Beta)

## Rust

1. Install Rust: [rustup](https://www.rust-lang.org/tools/install)
2. Install WebAssembly target:

```bash
rustup target add wasm32-wasi
```

## WasmEdge on Ubuntu

- [WasmEdge Installation](https://wasmedge.org/book/en/quick_start/install.html)

```bash
curl -sSf https://raw.githubusercontent.com/WasmEdge/WasmEdge/master/utils/install.sh | bash
source $HOME/.wasmedge/env
```

```bash
wasmedge -v

wasmedge version 0.11.2
```
