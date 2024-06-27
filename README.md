# icrc1-ledger
This is an icrc1 ledger implementation decoupled from https://github.com/dfinity/ic, based on changeset [9c006a50d364edf1403ef50b24c3be39dba8a5f6](https://github.com/dfinity/ic/releases/tag/release-2024-06-19_23-01-cycle-hotfix).

## Dependencies

```
../rs/bitcoin/types/internal

../rs/constants
../rs/utils
../rs/protobuf
../rs/phantom_newtype

../rs/crypto/internal/crypto_lib/sha2
../rs/crypto/internal/crypto_lib/types
../rs/crypto/sha2
../rs/crypto/tree_hash

../rs/rust_canisters/http_types
../rs/rust_canisters/canister_log

../rs/rosetta-api/icrc1/ledger
../rs/rosetta-api/ledger_core
../rs/rosetta-api/icrc1/tokens_u256
../rs/rosetta-api/icrc1/tokens_u64
../rs/rosetta-api/ledger_canister_core

../rs/rosetta-api/icrc1

../rs/types/error_types
../rs/types/base_types
../rs/types/management_canister_types

../packages/ic-ledger-hash-of
../packages/icrc-ledger-types
../packages/icrc-ledger-client
```

## Build 

```bash
cd rs/rosetta-api/icrc1/ledger
cargo build --target wasm32-unknown-unknown --profile canister-release --package ic-icrc1-ledger
```
