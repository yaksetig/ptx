# Rayls Privacy Token
In this page, we cover the entire privacy token of the Rayls platform. 

## System Architecture
Our architecture is simple, we assume a main EVM chain, which we call the "Commit Chain". Connected to each commit chain, there is a "Privacy Ledger". Each privacy ledger acts similarly to a validium L2. Therefore, each PL has a balance on the commit chain, but does not publish the transactions that take place internally. We note that, in our system, these balances are not public and are a feature of this privacy token.

We show below the architecture for a total of 5 financial institutions (i.e., relayers + privacy ledgers).

<p align="center">
  <img src="https://github.com/yaksetig/zktoken/blob/main/figures/rayls_architecture.png" />
</p>

## Protocols
The system is comprised of multiple protocols. Concretely, the network **setup step** where an admin kickstarts the network. Subsequently, we have a **registration phase** where financial institutions register their components (relayer and privacy ledger) in the network and obtain a wallet address in the network. Upon successful registration, there is a **key agreement step** where all the financials institutions obtain shared secrets with all the other financial institutions. Once all the shared secrets are established, the financial institutions must go through the **funding stage** for their accounts (either via an existing transparent balance or via the network admin). This step turns the balance into a Pedersen commitment. Upon successful funding of the account and now having a balance in the form of a commitment, institutions can now run the **private transaction protocol**, along with a **private message signaling protocol** . To receive these transactions, the financial institutions perform a **private information retrieval protocol** to download the block containing the transactions. To finalize the process, the recipients perform the opening and decryption of the transactions. 

### Setup
First, an admin must deploy the different contracts. 

### Registration
TBD

### Key Agreement
TBD

### Funding Account(s)
TBD

### Private Message Signaling

### Private Transfers
TBD

### Private Information Retrieval
TBD

### Private Lookup
TBD

### Transaction Opening
TBD


