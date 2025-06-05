# Repository Guidelines

When contributing to this project, please ensure the following steps are completed before committing changes:

1. Initialize the test data submodules if you haven't already:
   `git submodule update --init`.
2. Run the test suite with Cargo. You can run all tests or limit to specific crates, for example:
   `cargo test` or `cargo test -p arrow`.
3. Format all Rust code with rustfmt and verify no changes are required:
   `cargo +stable fmt --all -- --check`.
   The parquet crate requires an additional check:
   `cargo fmt -p parquet -- --check --config skip_children=true $(find ./parquet -name "*.rs" \! -name format.rs)`.
4. Run Clippy to ensure there are no lints:
   `cargo clippy --workspace --all-targets --all-features -- -D warnings`.
5. Optionally install the `pre-commit.sh` script as `.git/hooks/pre-commit` to automate these checks.

Following these steps keeps the codebase consistent with the guidance in `CONTRIBUTING.md`.
