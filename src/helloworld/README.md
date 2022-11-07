# Hello World

```bash
cargo new helloworld
```

```bash
rustup target add wasm32-wasi
```

```bash
cargo build --target wasm32-wasi
```

```bash
wasmedgec target/wasm32-wasi/debug/helloworld.wasm helloworld.wasm
```

```bash
wasmedge helloworld.wasm good morning

hello
good
morning
```
