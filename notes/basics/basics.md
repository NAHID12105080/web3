# Hierarchical Deterministic (HD) Wallets

HD wallets are wallets that generate a tree of keys from a single seed. This allows for the easy generation of new keys without the need to store them individually. HD wallets use BIP-32 for key generation, while standards like BIP-44 provide structure for different cryptocurrencies and accounts.

## Key Concepts

1. **Master Seed**: The starting point, derived from a mnemonic (usually 12 or 24 words).
2. **Derivation Path**: A structured path used to derive keys, typically following the format:

m / purpose' / coin_type' / account' / change / address_index

- **m**: Master key.
- **purpose'**: Usually `44'` for BIP-44 wallets.
- **coin_type'**: Specifies the cryptocurrency (e.g., `0'` for Bitcoin, `60'` for Ethereum).
- **account'**: Defines different user accounts (e.g., `0'`, `1'`, `2'`).
- **change**: `0` for external (receiving) addresses, `1` for internal (change) addresses.
- **address_index**: Index number of the specific key/address.

## Example: Ethereum HD Wallet Derivation Path

For an Ethereum wallet (coin type `60'`), with the third account:
m / 44' / 60' / 2' / 0 / 0


### Breakdown:
- **m**: Master key (derived from the seed).
- **44'**: Purpose (following BIP-44).
- **60'**: Ethereum's coin type.
- **2'**: Third account in the hierarchy.
- **0**: External (receiving) chain.
- **0**: First address in this account.

### Example in Ethereum:
m/44'/60'/2'/0/0
- This refers to the first receiving address of the third Ethereum account.


