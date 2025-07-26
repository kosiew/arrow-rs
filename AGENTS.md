# Repository Guidelines for Contributors

We value clear, maintainable, and high-performance Rust code. Prioritize delivering robust solutions—formatting and lint checks come at the end.

---

## 1. Solution-First Workflow

* **Understand Requirements:** Confirm scope and behavior before coding. Discuss edge cases and performance considerations if needed.
* **Design & Planning:** Outline the architecture, API ergonomics, and data flow. Draw diagrams or write pseudocode for complex changes.
* **Demonstrate Usage:** Add brief examples in doc comments or updated `README.md` sections.

## 2. Code Quality & Best Practices

### Idiomatic Rust

* Use `snake_case` for functions and variables; `CamelCase` for types and enums.
* Favor pattern matching, iterators, and zero-cost abstractions.
* Use `Option<T>` and `Result<T, E>` for optional values and fallible operations.

### Error Handling

* Propagate errors with the `?` operator. Avoid `unwrap()` and `expect()` in library code.
* Leverage `ArrowError` or `ParquetError` for domain-specific failures.
* Document all `unsafe` blocks with clear safety justifications.

### Performance Considerations

* Prefer zero-copy patterns (e.g., `&[u8]`) over owned allocations.
* Minimize cloning and heap usage; use `Arc<T>` for shared data.
* Consider SIMD or parallel iteration for compute-intensive paths.

## 3. Testing Strategy

* **Unit Tests:** Place alongside implementation with `#[cfg(test)]`. Cover edge cases and error paths.
* **Integration Tests:** Use the `tests/` directory to validate end-to-end behavior.
* **Benchmarks:** Put benchmarks in `benches/` and measure performance-critical code.
* **Test Data:** Initialize with `git submodule update --init` and reference the `parquet-testing` submodule where applicable.

## 4. Documentation & Examples

* Write `///` comments for public items with concise descriptions and code snippets.
* Use `//!` for module-level documentation and link to examples.
* Update project-level docs (`README.md`, `CONTRIBUTING.md`) to reflect new features.

## 5. Collaboration & Review

* **Pull Requests:** Include a clear summary of changes, rationale, and verification steps.
* **Code Reviews:** Focus on correctness, clarity, and performance. Offer constructive suggestions.
* **Issue Discussions:** Open issues for major design proposals or API changes.

## 6. Final Checks (Optional)

Run these after your solution is complete and tests pass:

```bash
# Initialize test data
git submodule update --init

# Run tests (all or specific crate)
cargo test               # or cargo test -p arrow

# Formatting checks
cargo +stable fmt --all -- --check\ RC="$(find ./parquet -name "*.rs" \! -name format.rs)" && \
  cargo fmt -p parquet -- --check --config skip_children=true $RC

# Linting
cargo clippy --workspace --all-targets --all-features -- -D warnings
```

Optionally, install the provided `pre-commit.sh` in `.git/hooks/` to automate these steps. Remember: clarity, correctness, and performance first—linting and formatting second. 🚀
