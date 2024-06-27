# icrc1-ledger
This is an icrc1 ledger implementation decoupled from https://github.com/dfinity/ic.

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
cargo build --release --target wasm32-unknown-unknown --package ic-icrc1-ledger
```
