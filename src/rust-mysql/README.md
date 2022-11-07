# Microservice with a database backend

- [Docker + Wasm](https://docs.docker.com/desktop/wasm/) (Beta)
- github: [second-state/microservice-rust-mysql](https://github.com/second-state/microservice-rust-mysql)

## Start Docker

```bash
docker compose up -d
```

### Usage

```bash
curl http://localhost:8080/init

{"status":true}
```

```bash
curl http://localhost:8080/create_orders -X POST -d @orders.json

{"status":true}
```

```bash
curl http://localhost:8080/orders

[{"order_id":1,"product_id":12,"quantity":2,"amount":56.0,"shipping":15.0,"tax":2.0,"shipping_address":"Mataderos 2312"},{"order_id":2,"product_id":15,"quantity":3,"amount":256.0,"shipping":30.0,"tax":16.0,"shipping_address":"1234 NW Bobcat"},{"order_id":3,"product_id":11,"quantity":5,"amount":536.0,"shipping":50.0,"tax":24.0,"shipping_address":"20 Havelock"},{"order_id":4,"product_id":8,"quantity":8,"amount":126.0,"shipping":20.0,"tax":12.0,"shipping_address":"224 Pandan Loop"},{"order_id":5,"product_id":24,"quantity":1,"amount":46.0,"shipping":10.0,"tax":2.0,"shipping_address":"No.10 Jalan Besar"}]
```

```bash
curl http://localhost:8080/update_order -X POST -d @update_order.json

{"status":true}

#    {"order_id":3,"product_id":11,"quantity":5,"amount":536.0,"shipping":50.0,"tax":24.0,"shipping_address":"20 Havelock"}
# -> {"order_id": 3,"product_id": 12,"quantity": 2,"amount": 56,"shipping": 15,"tax": 2,"shipping_address": "123 Main Street"}
```

```bash
curl http://localhost:8080/delete_order?id=2

{"status":true}
```

```bash
curl http://localhost:8080/orders

[{"order_id":1,"product_id":12,"quantity":2,"amount":56.0,"shipping":15.0,"tax":2.0,"shipping_address":"Mataderos 2312"},{"order_id":3,"product_id":12,"quantity":2,"amount":56.0,"shipping":15.0,"tax":2.0,"shipping_address":"123 Main Street"},{"order_id":4,"product_id":8,"quantity":8,"amount":126.0,"shipping":20.0,"tax":12.0,"shipping_address":"224 Pandan Loop"},{"order_id":5,"product_id":24,"quantity":1,"amount":46.0,"shipping":10.0,"tax":2.0,"shipping_address":"No.10 Jalan Besar"}]
```

---

## Run a rust app in local

### Install Rust

1. Install Rust: [rustup](https://www.rust-lang.org/tools/install)
2. Install WebAssembly target:

```bash
rustup target add wasm32-wasi
```

### Install WasmEdge

- Install [Homebrew](https://brew.sh/)
- Install Libs: `brew install llvm zlib pkg-config`
- [WasmEdge Installation](https://wasmedge.org/book/en/quick_start/install.html)

```bash
curl -sSf https://raw.githubusercontent.com/WasmEdge/WasmEdge/master/utils/install.sh | bash
```

```bash
wasmedge -v

wasmedge version 0.11.2
```

### Run with wasm

```bash
cargo build --target wasm32-wasi --release
```

Output:

```bash
target/wasm32-wasi/release/
├── build/
├── deps/
├── examples/
├── incremental/
├── order_demo_service.d
└── order_demo_service.wasm*
```

Optimize:

```bash
wasmedgec target/wasm32-wasi/release/order_demo_service.wasm order_demo_service.wasm

[2022-11-04 13:54:47.175] [info] compile start
[2022-11-04 13:54:47.899] [info] verify start
[2022-11-04 13:54:48.287] [info] optimize start
[2022-11-04 13:55:47.901] [info] codegen start
[2022-11-04 13:56:46.105] [info] output start
lld: warning: …/wasm/src/rust-mysql/order_demo_service.a0215eba7c.6fe52b4813.o has version 12.0.0, which is newer than target minimum of 10.0
[2022-11-04 13:56:46.238] [info] compile done
[2022-11-04 13:56:46.372] [info] output start
```

Run:

```bash
wasmedge --env "DATABASE_URL=mysql://root:whalehello@localhost:3306/mysql" order_demo_service.wasm
```
