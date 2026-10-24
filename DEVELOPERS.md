# TODO: link to here from sphinx docs

## Installing from source

### Fetch submodules
git submodule update --recursive --init

### Install with dev dependencies
uv pip install -e '.[dev]'

This includes all dependencies for building, linting, testing, and checking in code.
If you optionally want to install only a subset of dev tools, replace 'dev' with the following flag:

build: package release deps
docs: documentation tools
lint: code linting, fixing, and type checking
test: unit and integration tests

## Miscellaneous

### Setting up precommit hooks
pre-commit install

### Building documentation
sphinx-build -M html source build -a

### Building standalone release binaries
1. Set up the build target:
   - x86_64: `rustup target add x86_64-unknown-linux-musl`
   - aarch64: `rustup target add aarch64-unknown-linux-musl`
2. Run `python3 scripts/release.py`, optionally setting the target explicitly:
   - `DZGUI_TARGET=x86_64-unknown-linux-musl DZGUI_PYTHON_TARGET=x86_64-unknown-linux-gnu python3 scripts/release.py`
   - `DZGUI_TARGET=aarch64-unknown-linux-musl DZGUI_PYTHON_TARGET=aarch64-unknown-linux-gnu python3 scripts/release.py`
3. Generate a release and tag against the resulting tarfile generated in `dist`

Both targets default to x86_64 when the environment variables are omitted.

### Publishing release binaries
Pushing a version tag (for example `v7.0.0` or `7.0.0b28`) runs the
`Build and publish release` workflow. It builds both x86_64 and aarch64
tarballs on native GitHub runners and attaches them to the GitHub release.

Pushing the `feat/aarch64-release` branch builds and uploads both tarballs as
workflow artifacts without creating a release. This can be used to validate the
build before tagging.

The workflow can also be started manually with an existing tag via the
`workflow_dispatch` input once it exists on the default branch.
