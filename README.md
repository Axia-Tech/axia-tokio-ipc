# axia-tokio-ipc

[![CI](https://github.com/axia-tech/axia-tokio-ipc/actions/workflows/ci.yml/badge.svg)](https://github.com/axia-tech/axia-tokio-ipc/actions/workflows/ci.yml)
[![Documentation](https://docs.rs/axia-tokio-ipc/badge.svg)](https://docs.rs/axia-tokio-ipc)

This crate abstracts interprocess transport for UNIX/Windows.

It utilizes unix sockets on UNIX (via `tokio::net::UnixStream`) and named pipes on windows (via `tokio::net::windows::named_pipe` module).

Endpoint is transport-agnostic interface for incoming connections:
```rust
use axia_tokio_ipc::Endpoint;
use futures::stream::StreamExt;

// For testing purposes only - instead, use a path to an actual socket or a pipe
let addr = axia_tokio_ipc::dummy_endpoint();

let server = async move {
    Endpoint::new(addr)
        .incoming()
        .expect("Couldn't set up server")
        .for_each(|conn| async {
            match conn {
                Ok(stream) => println!("Got connection!"),
                Err(e) => eprintln!("Error when receiving connection: {:?}", e),
            }
        });
};

let rt = tokio::runtime::Builder::new_current_thread().enable_all().build().unwrap();
rt.block_on(server);
```


# License

`axia-tokio-ipc` is primarily distributed under the terms of both the MIT
license and the Apache License (Version 2.0), with portions covered by various
BSD-like licenses.

See LICENSE-APACHE, and LICENSE-MIT for details.
