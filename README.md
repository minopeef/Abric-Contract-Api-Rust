# Fabric Rust Contract-API

The Fabric Contract API in Rust, with WebAssembly (Wasm) as the compilation target.

A Smart Contract is a single crate, containing one or more contract structs, compiled to a Wasm library. In this repository, `basic_contract_rs` is an example of a simple asset-based contract.

## Technology Preview

NOTE: This is a technology preview, rather than production ready. Released for feedback and community experimentation.

## Project Structure

This workspace contains the following crates:

- `fabric_contract`: Core library providing the Hyperledger Fabric Contract API in Rust
- `fabric_contract_macros`: Procedural macros for contract development
- `basic_contract_rs`: Example basic asset contract with CRUD operations
- `asset_transfer_rs`: Example asset transfer contract
- `asset_transfer_secure_private_rs`: Example secure private asset transfer contract with endorsement policies

## Quick Start

### Prerequisites

- Rust stable toolchain installed
- WebAssembly target added: `rustup target add wasm32-unknown-unknown`

Note: wasm-pack is not required here as there is no JavaScript host.

### Building for WebAssembly

To build a Wasm binary for a contract:

```
cargo build --target wasm32-unknown-unknown
```

The compiled Wasm file will be located at:
`target/wasm32-unknown-unknown/debug/[contract_name].wasm`

For release builds with optimizations:

```
cargo build --target wasm32-unknown-unknown --release
```

### Writing and Deploying a Smart Contract

At a high level, the steps are:

1. As the crates are not published yet, clone this entire cargo workspace to build both the fabric_contract and your own crates
2. Write the Smart Contract in Rust (see `basic_contract_rs` for an example)
3. Compile targeting wasm: `cargo build --target wasm32-unknown-unknown`
4. Take the resulting Wasm binary and use the fabric-chaincode-wasm project to encapsulate this Wasm file in the Wasm Chaincode Runtime
5. Once deployed, interact with this like any other contract

### Building Documentation

To build and view the API documentation:

```
just docs
```

Or manually:

```
cargo doc --no-deps --open
```

## Development Tools

This project includes a `justfile` for quick command execution. Available commands:

- `just wasm`: Build for WebAssembly target
- `just amd64`: Build for native target
- `just docs`: Build and copy documentation
- `just expand`: Expand macros for debugging (requires cargo-expand)
- `just clippy`: Run clippy linter
- `just fmt`: Format all code
- `just clean`: Clean build artifacts
- `just all`: Run clean, wasm, fmt, clippy, and docs

## Code Optimizations

The project has been optimized for performance and code quality:

- Removed unused files and dead code
- Optimized string allocations (replaced unnecessary `format!()` calls with `.to_string()`)
- Simplified error handling patterns using `?` operator and `map_err`
- Reduced unnecessary clones and redundant operations
- Improved code readability and maintainability

## Contract Examples

### Basic Contract

The `basic_contract_rs` example demonstrates:
- Creating assets
- Reading assets
- Checking asset existence
- Counting assets

### Asset Transfer Contract

The `asset_transfer_rs` example demonstrates:
- Initializing ledger with sample data
- Creating, reading, updating, and deleting assets
- Transferring asset ownership
- Query operations

### Secure Private Asset Transfer

The `asset_transfer_secure_private_rs` example demonstrates:
- Private data collections
- Endorsement policies
- Organization-specific data storage
- Asset verification and transfer with price agreements

## License

Licensed under the Apache-2.0 license.
