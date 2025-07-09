# GitHub Copilot Instructions for Apache Arrow Rust

This repository contains the official native Rust implementation of Apache Arrow and Apache Parquet. These instructions help GitHub Copilot provide better assistance when working on this codebase.

## Project Overview

This is a Rust workspace containing multiple crates:

- **arrow**: Core Arrow functionality (memory layout, arrays, computations)
- **arrow-\***: Various Arrow-related crates (flight, ipc, csv, json, etc.)
- **parquet**: Parquet columnar file format implementation
- **parquet_derive**: Derive macros for Parquet serialization

## Code Style and Conventions

### Rust-Specific Guidelines

- Follow the project's `rustfmt.toml` configuration for formatting
- Use `cargo clippy` for linting - the project has specific clippy configurations
- Prefer `unsafe` blocks only when absolutely necessary and always document safety requirements
- Use appropriate error handling with `Result<T, E>` types
- Follow Rust naming conventions (snake_case for functions/variables, PascalCase for types)

### Arrow/Parquet Specific Patterns

- Use zero-copy operations where possible for performance
- Prefer strongly-typed APIs over generic ones when working with Arrow data types
- When working with arrays, use the appropriate `ArrayData` and `Buffer` abstractions
- For Parquet, leverage the schema and metadata APIs appropriately
- Use `ArrowError` and `ParquetError` for error handling within respective crates

### Performance Considerations

- This is a high-performance library - always consider memory allocation patterns
- Use SIMD operations where available and beneficial
- Prefer batch operations over element-by-element processing
- Consider cache locality when designing data structures
- Profile before optimizing, but be aware of common performance pitfalls

## Testing Guidelines

### Test Structure

- Unit tests should be in the same file as the code (`#[cfg(test)]` modules)
- Integration tests go in the `tests/` directory
- Benchmarks go in the `benches/` directory
- Use property-based testing with `proptest` for complex data structures

### Test Data

- Use the `parquet-testing` submodule for test data when available
- Generate minimal test cases that demonstrate specific functionality
- Include both positive and negative test cases
- Test edge cases (empty data, very large data, malformed inputs)

### Cross-Platform Testing

- Be aware of endianness issues when working with binary data
- Consider different architectures (x86, ARM, etc.)
- Test on different operating systems when platform-specific code is involved

## Documentation Standards

### Code Documentation

- All public APIs must have rustdoc comments with examples
- Use `///` for public API documentation
- Use `//!` for module-level documentation
- Include safety documentation for any `unsafe` code
- Reference relevant Arrow/Parquet specifications where applicable

### Examples

- Provide runnable examples in rustdoc comments
- Include examples in the `examples/` directories
- Show both basic usage and advanced patterns
- Demonstrate integration between different crates when relevant

## Dependencies and Features

### Dependency Management

- Minimize external dependencies where possible
- Use workspace dependencies defined in the root `Cargo.toml`
- Prefer well-maintained crates with good security track records
- Be mindful of MSRV (Minimum Supported Rust Version) constraints

### Feature Flags

- Use feature flags to make optional dependencies truly optional
- Default features should provide a good out-of-the-box experience
- Document feature flags clearly in README files
- Test both with and without optional features enabled

## Apache Software Foundation Guidelines

### Licensing

- All new files must include the Apache License header
- Use the exact header format found in existing files
- Ensure all dependencies are compatible with Apache License 2.0

### Attribution

- Credit external code sources appropriately
- Update NOTICE.txt files when adding significant external code
- Follow ASF guidelines for third-party contributions

## Common Patterns to Follow

### Error Handling

```rust
// Use specific error types
use arrow::error::ArrowError;
use parquet::errors::ParquetError;

// Return Result types for fallible operations
fn process_data() -> Result<Vec<u8>, ArrowError> {
    // implementation
}
```

### Memory Management

```rust
// Use Buffer for zero-copy operations
use arrow::buffer::Buffer;

// Prefer owned data when sharing across threads
use std::sync::Arc;
```

### Type Safety

```rust
// Use phantom types for compile-time guarantees
use std::marker::PhantomData;

// Leverage the type system to prevent invalid states
```

## What to Avoid

- Don't use `unwrap()` or `expect()` in library code - always handle errors properly
- Avoid unnecessary allocations in hot paths
- Don't expose internal implementation details in public APIs
- Avoid breaking changes to public APIs without proper deprecation
- Don't add platform-specific code without fallbacks
- Avoid complex generic constraints that hurt compile times or readability

## Integration Points

When working across crates:

- Use the `arrow` crate's type system as the foundation
- Ensure `parquet` and `arrow` interoperability is seamless
- Consider how changes affect the entire ecosystem
- Test integration scenarios, not just individual crate functionality

## Performance Benchmarking

- Use `criterion` for benchmarks
- Include realistic workloads in benchmarks
- Compare against established baselines
- Consider both throughput and latency metrics
- Profile memory usage patterns

This is a high-performance, production-grade library used by many organizations. Please prioritize correctness, performance, and API stability when making suggestions.
