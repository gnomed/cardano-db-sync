# Agents Guide: cardano-db-sync

## Critical Warnings
- **Avoid `master` branch**: Always build and run from the latest release tag.
- **Strict GHC Options**: All packages use `-Wall -Werror`. No warnings are permitted; all must be fixed before committing.

## Project Structure
- `cardano-db`: Database schema and common Haskell data types.
- `cardano-db-sync`: Main node; follows Cardano chain and populates PostgreSQL.
- `cardano-db-tool`: Database management (migrations, validation).
- `cardano-smash-server`: Server for interacting with the DB.
- `cardano-chain-gen`: Chain generation utilities.

## Build & Run
- **Toolchain**: Use Nix (recommended) or Cabal.
- **Build (Nix)**: `nix build -v .#cardano-db-sync -o db-sync-node`
- **Build (Cabal)**: `cabal build cardano-db-sync`
- **Database Setup**: `PGPASSFILE=config/pgpass-mainnet scripts/postgresql-setup.sh --createdb`
- **Execution**: `PGPASSFILE=config/pgpass-mainnet cabal run cardano-db-sync -- --config config/mainnet-config.yaml --socket-path ../cardano-node/state-node-mainnet/node.socket --state-dir ledger-state/mainnet --schema-dir schema/`
- **Prerequisite**: `secp256k1` library must be installed via `scripts/secp256k1-setup.sh`.

## Code Quality
- **Formatting**: Run `scripts/fourmolize.sh` (uses `fourmolu`).
- **Linting**: Run `hlint` (integrated in `scripts/git-pre-commit-hook`).
- **Schema Validation**: Use `scripts/run-schema-checks.sh <dbname>` to verify referential integrity and uniqueness.

## Infrastructure
- **Database**: PostgreSQL.
- **Connection**: `cardano-db-sync` connects to a local `cardano-node` via a Unix domain socket.
