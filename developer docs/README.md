# Understanding Rayls
TBD

## Privacy Ledger (Prover)
The privacy ledger is basically a custom Geth client with no consensus. The main part here is to ensure that a specific trigger exists for the relayer to know that a cross-chain transaction should take place. 

## Relayer

### Golang Transaction Creator

### Circom Prover


## Commit Chain
The commit chain is an EVM-compatible blockchain. Concretely, it is an instatiation of Hyperledger Besu. Our system, however, supports any type of underlying EVM blockchain (e.g., Polygon, Ethereum, Avalanche, ...). 

### Private Token Contract
Inside the commit chain lives a private token contract. 

#### Verifier Token Contract
TBD
