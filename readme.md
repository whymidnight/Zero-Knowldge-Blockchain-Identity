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
> Generate the proof on A/B Chain, verify on B/A Chain, viola.
Right? No. There does _not_ seem to be a way to generate proofs onchain. Only zk verification seems to be implemented onchain - at least for Ethereum and Solana.

### Technique
> We need to prove the underlying state transition (transaction signature) that supports a proof actually happened.
Unfortunately Ethereum does _not_ have a way to do this without a proxied function in a (new) smart contract. If we suppose ERC-20 tokens, we can deploy a simple and minimal smart contract that _just_ calls `transfer()` and emits a _memo_.

It would be an ill implementation to assume any proof generated is valid without a disparate verification entity. Especially if proof generation happens client-side (which may not be feasible).

To satisfy efficacy of proof generation, the _memo_ of such state transition needs to be yielded from the chain. We need some sort of encryption/hashing function of _memo's_ to guarantee this clause.

#### End-to-End Guarantee's
We need to guarantee that across origin and destination chains, we have asserted the following 


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
