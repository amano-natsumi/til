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

check
cargo check
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


## rustコードの概念、用語
Package
    ├─bin crate― module ― sub module
                  ➯ ファイル単位 階層で定義
    │           実際に処理を実行する
    └─lib crate ― module
        単体では実行できない(他から呼び出される)
        
ライブラリサイト
crate.io

githubに公開もできる

lib crate作る時
carge new -- lib name

main.rs

