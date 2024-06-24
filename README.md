# Rayls Privacy Token
In this document, we cover the Rayls platform. 

## Problem Statement
The most popular commercial bank in India has 500M users. There is no blockchain that can even support this type of volume. The situation gets worse when you consider that these transactions should be private. Moreover, the commercial banks should also be able to perform transactions (e.g., settlements) using blockchain. Similarly, these banks also have privacy requirements. 

Solving this problem is particularly hard. First, the balance sheet of all banks must be private. Second, the transaction data from users must also be private. Third, the system should be performant (i.e., thousands of transactions per second). Last, the system should be auditable for compliance purposes. Additionally, the system should ideally be quantum-ready such that advances in the quantum-computing realm do not compromise the entire past transaction history. 

## Goals
The goal of Rayls is to allow for financial institutions to be on blockchain rayls (hence the name). 

While these goals may seem mutually exclusive and somewhat impossible to attain, we show how to address them efficiently. 

## System Architecture
Our architecture is simple, we assume a main EVM chain, which we call the "Commit Chain". Connected to each commit chain, there is a "Privacy Ledger". Each privacy ledger acts similarly to a validium L2. Therefore, each PL has a balance on the commit chain, but does not publish the transactions that take place internally. We note that, in our system, these balances are not public and are a feature of this privacy token.

We show below the architecture for a total of 5 financial institutions (i.e., relayers + privacy ledgers).

<p align="center">
  <img src="https://github.com/yaksetig/zktoken/blob/main/figures/rayls_architecture.png" />
</p>





## Protocols
The system is comprised of multiple protocols. Concretely, the network **setup step** where an admin kickstarts the network. Subsequently, we have a **registration phase** where financial institutions register their components (relayer and privacy ledger) in the network and obtain a wallet address in the network. Upon successful registration, there is a **key agreement step** where all the financials institutions obtain shared secrets with all the other financial institutions. Once all the shared secrets are established, the financial institutions must go through the **funding stage** for their accounts (either via an existing transparent balance or via the network admin). This step turns the balance into a Pedersen commitment. Upon successful funding of the account and now having a balance in the form of a commitment, institutions can now run the **private transaction protocol**, along with a **private message signaling protocol** . To receive these transactions, the financial institutions perform a **private information retrieval protocol** to download the block containing the transactions. To finalize the process, the recipients perform the opening and decryption of the transactions. 

### Setup
First, an admin must kickstart the network and deploy the different contracts. For example, to have private transactions in the network, the admin must deploy the Rayls private token contract to the commit chain. 

### Registration
We assume two keypairs in the system. One is "EVM-Compatible" (i.e., ECDSA). The other is a post-quantum keypair (e.g., CSIDH)

<p align="center">
  <img src="https://github.com/yaksetig/zktoken/blob/main/figures/key_registration.png" />
</p>

### Key Agreement
Upon successful registration, the relayer must download from the commit chain the keys from all the other relayers to obtain a (different) shared secret with each. Therefore, in a network with N relayers, a total of N-1 agreements is performed by each relayer. (N-1 because the relayer does not need to do an agreement with themselves, so that's one less key).

<p align="center">
  <img src="https://github.com/yaksetig/zktoken/blob/main/figures/key_agreement.png" />
</p>

### Funding Account(s)
TBD

<p align="center">
  <img src="https://github.com/yaksetig/zktoken/blob/main/figures/initial_balances.png" />
</p>

### Private Message Signaling

### Private Transfers
TBD

<p align="center">
  <img src="https://github.com/yaksetig/zktoken/blob/main/figures/rayls_send.png" />
</p>

### Private Information Retrieval
TBD

### Private Lookup
TBD

### Transaction Opening
TBD


