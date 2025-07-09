# About this file

This file provides essential information for the Gemini AI agent to effectively assist with development tasks in the `arrow-rs` repository. It outlines the project structure, key commands, and established conventions.

# Project Overview

This repository contains the official Rust implementation of Apache Arrow and Apache Parquet. It is a large, multi-crate workspace that provides tools for in-memory columnar data processing.

# Project Structure

The project is a Rust workspace with numerous crates. The primary crates are:
-   `arrow`: Core Arrow functionality, including memory layouts, array structures, and computational kernels.
-   `arrow-flight`: Support for the Arrow Flight RPC protocol.
-   `parquet`: Support for reading and writing the Parquet columnar file format.
-   `parquet_derive`: A crate for deriving `RecordWriter`/`RecordReader` for simple structs.

The full list of workspace members can be found in the root `Cargo.toml` file.

# Development Workflow

The following commands are standard for building, testing, and maintaining the codebase.

## Initial Setup

The repository uses git submodules for test data. Before running tests, initialize them:
```bash
git submodule update --init
```

## Building

Build the entire workspace using the standard Cargo command:
```bash
cargo build
```

To build a specific crate within the workspace:
```bash
cargo build -p <crate-name>
```

## Testing

Run all unit and integration tests:
```bash
cargo test
```

To run tests for a specific crate (e.g., `arrow`):
```bash
cargo test -p arrow
```

To run tests with all features for a crate:
```bash
cargo test -p arrow --all-features
```

## Formatting

The project uses `rustfmt` to ensure consistent code style. To check for formatting issues:
```bash
cargo +stable fmt --all -- --check
```

## Linting

The project uses `clippy` to catch common mistakes and improve code quality. To run the linter:
```bash
cargo clippy --workspace --all-targets --all-features -- -D warnings
```

# Committing and Contributing

The repository uses pre-commit hooks to automate checks. See `.pre-commit-config.yaml` for the specific hooks. For detailed contribution guidelines, including how to handle breaking changes, please refer to `CONTRIBUTING.md`.
