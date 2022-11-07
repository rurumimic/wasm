# Example

- src/wasm-example/[docker-compose.yml](src/wasm-example/docker-compose.yml)

```bash
cd src/wasm-example
docker compose up -d
```

```log
wasm-example | [2022-11-04 01:54:16.355] [error] loading failed: malformed section id, Code: 0x25
wasm-example | [2022-11-04 01:54:16.355] [error]     AOT arch type unmatched.
wasm-example | [2022-11-04 01:54:16.355] [error]     Load AOT section failed. Use interpreter mode instead.
wasm-example | Server is now running
```

```bash
curl localhost:8080

Hello world from Rust running with Wasm! Send POST data to /echo to have it echoed back to you
```

```bash
curl localhost:8080/echo -d '{"message":"Hi there"}' -H "Content-type: application/json"

{"message":"Hi there"}
```

```bash
docker compose down -v
```
