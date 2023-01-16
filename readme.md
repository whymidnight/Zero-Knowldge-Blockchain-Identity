# Web3 Auth

## Philosophy
Decentralise web3 identity authenticity by distributing authentication across different blockchains through Zero Knowledge Proofs.

## Preamble
Digital assets exist on many different blockchains - Ethereum, Solana, Polygon, Cardano, to name a few. DAO's leverage these digital assets to shape outcomes via attributing communal ownership through asset holdership. However in the current universe, there is technical complexity and/or technical impossibility to natively assimilate disparate blockchain technologies with cryptographical consistency. Thus, yielding an _entrusted_ implementation of bridging identities.

This project seeks to prototype a open sourced Zero Knowledge solution for identity authenticity.

## Resources
- [ConsenSys/gnark](https://github.com/ConsenSys/gnark)
- [zkLinkProtocol/groth16-sol-verifier](https://github.com/zkLinkProtocol/groth16-sol-verifier)

## Initial Brain Dump
Generate the proof on A/B Chain, verify on B/A Chain, viola.

### Proof Circuits
#### Asset Ownership
```go
type AssetOwnership struct {
    Amount             String
    Mint               String
    Owner              String
    ChainMemoSignature String
    ChainEnvironment   String // mainnet, testnet, devnet, localnet
}
```
_Asset Ownership_ needs to be confirmed on native chain via a memo transaction that proves ownership - this transaction signature gets attributed to the proof along with the amount of tokens held, token mint, and invoker/owner.

#### Asset Spend
```go
type AssetSpend struct {
    Amount              String
    Mint                String
    Owner               String
    ChainSpentSignature String
    ChainEnvironment    String // mainnet, testnet, devnet, localnet
}
```
_Asset Spend_ needs to be confirmed on native chain via a token burn transaction that proves tokens burned - this transaction signature gets attributed to the proof along with the amount of tokens burned, token mint, and invoker/owner.

### Proof generation with `gnark`
`gnark` is a Golang library - thus, imposing an API backend for proof generation _or_ WASM in the browser. Go WASM binaries are generally big; which can make user experiences suboptimal due do downloading many MB's worth of data.
