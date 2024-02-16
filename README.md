# WebAssembly with Rust Tutorial

A very simple starter tutorial for building WebAssembly with Rust that uses:
- Node.js for the basic web app files and local webserver
- Rust programming language for the WebAssembly
- wasm-pack for building the WebAssembly

This is a "hello-world" app based on one from [mozilla.org](https://developer.mozilla.org/en-US/docs/WebAssembly/Rust_to_Wasm)

## Overview

This tutorial will show you how to build a simple WebAssembly module using Rust 
and then use it in a web application. It assumes the most basic knowledge of Rust and 
web development. The tutorial will cover the following steps:

1. Install Node.js, npm and http-server
2. Install Rust
3. Install wasm-pack
4. Create a new Rust project
5. Configure the Rust project to dynamic library binary
6. Create the WebAssembly module with wasm-pack
7. Create a index page and add the code to use the WebAssembly module
8. Start the web server

## Prerequisites

- Node.js
- npm
- node webserver (http-server) 
- Rust

### Install Node.js, npm and http-server

```bash
sudo apt update
sudo apt install nodejs
sudo apt install npm
npm install -g http-server
```

### Install Rust

Install Rust using rustup, a toolchain manager for Rust.
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Follow the instructions to install Rust. Once installed, you can verify the installation.

## Getting Started

First, install the wasm-pack tool, which will be used to build the WebAssembly module.
### Install wasm-pack

```bash
cargo install wasm-pack
```

## Create a new Rust project
Create a new project with the following command:

```bash
cargo new --lib hello-wasm
```

This will create a new directory called `hello-wasm` with the following structure:

```bash
cd hello-wasm
tree
.
├── Cargo.toml
└── src
    └── lib.rs
```

### Update Cargo.toml

Add the following to the `Cargo.toml` file:

```toml
[lib]
crate-type = ["cdylib"]
```

This will tell Rust to build the project as a dynamic library.

### Update lib.rs

Replace the contents of `src/lib.rs` with the following:

```rust
use wasm_bindgen::prelude::*;

#[wasm_bindgen]
// This will export the greet function to JavaScript
extern {
    pub fn alert(s: &str);  // alert function from browser
}

// This public function  will be the entry point for the WebAssembly module
#[wasm_bindgen]
pub fn greet(name: &str) {
    alert(&format!("Hello, {}!", name));
}
```

### Build the WebAssembly module

Build the WebAssembly module with the following command:

```bash
wasm-pack build
```

This will create a new directory called `pkg` with the following structure:

```bash
tree pkg
pkg
├── hello_wasm_bg.wasm
├── hello_wasm_bg.wasm.d.ts
├── hello_wasm.d.ts
├── hello_wasm.js
└── package.json

0 directories, 5 files
```

### Create a index page

Create `index.html` with the following:

```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <title>hello-wasm example</title>
  </head>
  <body>
    <script type="module">
      import init, { greet } from "./pkg/hello_wasm.js";
      init().then(() => {
        greet("WebAssembly");
      });
    </script>
  </body>
</html>
```

### Start the web server

Start the web server with the following command:

```bash
http-server
```

This will start the web server on port 8080. Open your web browser and navigate to `http://localhost:8080`. You should see an alert with the message "Hello, WebAssembly!".
