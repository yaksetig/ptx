# Understanding Rayls
TBD

## Privacy Ledger (Prover)
The privacy ledger is basically a custom Geth client with no consensus. The main part here is to ensure that a specific trigger exists for the relayer to know that a cross-chain transaction should take place. We note that for the users (i.e., clients of financial institutions), the flow remains the exact same and nothing changes. 

## Relayer
The relayer reads events from the Privacy ledger and performs action on the commit chain (or vice-versa). 

### Golang Transaction Creator
There is an additional piece of Golang code that receives key material from the privacy ledger and relayer and generates the Pedersen commitments for a PTX transaction. These values are then passed onto a circom prover that generates the zero-knowledge proof that the transaction is correct.

### Circom Prover
The Relayer includes an additional module (or component) internally that allows for the creation of zero-knowledge proofs for PTX transactions.

Upon successfully generating the Pedersen commitments and the zero-knowledge proof, these values can be sent to the PTX smart contract on the commit chain.


## Commit Chain
The commit chain is an EVM-compatible blockchain. Concretely, it is an instatiation of Hyperledger Besu. Our system, however, supports any type of underlying EVM blockchain (e.g., Polygon, Ethereum, Avalanche, ...). 

### PTX (Private Transactions) Contract
Inside the commit chain lives a private token contract that contains the logic to check the nullifier, add Pedersen commitments. To check the correctness of the zero-knowledge proof, the PTX contract calls the verifier contract.  

#### Verifier Contract
This is a contract exported by Circom and deployed in the setup phase. This contract receives a zk proof and returns if the proof is correct or not. This acts as a crucial check to ensure the correct operation of the PTX contract. 
