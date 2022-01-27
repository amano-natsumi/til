# Rust

- src下にコード置く

```
.
├── Cargo.lock
├── Cargo.toml
├── README.md
├── src
│   └── main.rs
└── target
    ├─ {build file}
```



```
build + run
cargo run

buildのみ
cargo build
```

リリース用build

```
cargo build --release
```

プロジェクト作成
```
cargo new hello_world --bin
```

## Cargo.toml

```
[package]

name = "hello_world"
version = "0.0.1"
authors = [ "amano <you@example.com>" ]
```
