# ICRC1 Ledger

This is an [ICRC1](https://github.com/dfinity/ICRC-1) ledger implementation decoupled from [IC](https://github.com/dfinity/ic), based on changeset [9c006a50d364edf1403ef50b24c3be39dba8a5f6](https://github.com/dfinity/ic/releases/tag/release-2024-06-19_23-01-cycle-hotfix).

The reason of doing this is you can have a light-weight reference code of the ICRC1 standard.

## Methodology

There're several things I followed during the decoupling:

- In order to keep the work as simple as possible, I kept the original directory hierachy, as there're many relative path references in the .toml files.
- The `cargo tree` command can be used to show the dependency graph of the `ic-icrc1-ledger` package.  
  You can also run `cargo build` to check the compiled crates to get the dependencies.
- Removed the tests from the build as `ic-icrc1-ledger-sm-tests` references `ic-state-machine-tests`, which references almost other crates in the IC project.  
  If you run `cargo tree`, it will generate about `2000` lines as the depdency graph of `ic-state-machine-tests`.

## Dependencies

The below are the necessary dependencies for the `ic-icrc1-ledger` package.

```
packages/ic-ledger-hash-of
packages/icrc-ledger-client
packages/icrc-ledger-types

rs/bitcoin/types/internal

rs/constants

rs/crypto/internal/crypto_lib/sha2
rs/crypto/internal/crypto_lib/types
rs/crypto/sha2
rs/crypto/tree_hash

rs/phantom_newtype
rs/protobuf

rs/rust_canisters/canister_log
rs/rust_canisters/http_types

rs/rosetta-api/icrc1
rs/rosetta-api/icrc1/ledger
rs/rosetta-api/icrc1/tokens_u256
rs/rosetta-api/icrc1/tokens_u64
rs/rosetta-api/ledger_canister_core
rs/rosetta-api/ledger_core

rs/types/base_types
rs/types/error_types
rs/types/management_canister_types

rs/utils
```

I believe there're some dependencies can be removed from the list, for example `rs/bitcoin/types/internal` which is only remained as it's refenreced by `rs/types/management_canister_types`. But this needs to touch the code, which is hard to maintain(For example if you want to upgrade to a new revision in the IC repo).

So I simply keep them, and rely on the [ic-wasm](https://github.com/dfinity/ic-wasm) to remove unused code. Not sure if it's the best choice, but let it be now.

## Build 

You can build the ICRC1 ledger as below:

```bash
cargo build --target wasm32-unknown-unknown --profile canister-release --package ic-icrc1-ledger
```

The output Wasm file can be found at `./target/wasm32-unknown-unknown/canister-release` directory. 

And you can use [ic-wasm](https://github.com/dfinity/ic-wasm) to optimize the Wasm code.

```bash
cargo install ic-wasm
ic-wasm target/wasm32-unknown-unknown/canister-release/ic-icrc1-ledger.wasm -o icrc1-ledger.wasm shrink
```

Below is a table about build size, comparing to building from [IC](https://github.com/dfinity/ic) with changeset [9c006a50d364edf1403ef50b24c3be39dba8a5f6](https://github.com/dfinity/ic/releases/tag/release-2024-06-19_23-01-cycle-hotfix).

|         Repo            | Before optimization   | After optimization |
|------------------------ | :-------------------: | :----------------: |
| From icrc1-ledger repo  | 2,116 KB              | 1,346 KB           |
| From IC repo            | 1,665 KB              | 1,343 KB           |

As you can see the Wasm code built from this repo is bigger than the one built from the IC repo. Not quite sure the root cause, but apparently some unnecesary dependencies are packed into the build. 

But fortunately, the Wasm code size is pretty much the same after shrinking by `ic-wasm`.
