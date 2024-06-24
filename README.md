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
We assume two different types of balances per each private token in the network. The main one lives on the commit chain. The second one, however, is a wrapped version and lives inside each privacy ledger. Minting on the commit chain is only allowed by the administrator. Minting inside each privacy ledger is of the responsability of the owner of the privacy ledger. The amount inside the privacy ledger must match the amount on the commit chain. 

Therefore, the process of funding accounts begins on the commit chain via the mint from the admin or via the transfer of "transparent" (e.g., ERC-20) funds to the private token contract on the main chain. During this process, the balance gets converted from a uint256 into a Pedersen commmitment (i.e., elliptic curve point). We note that whenever the private account is funded, the amount that is sent to the contract is public to anyone that has visibility to the blocks on the commit chain. However, after one single transfer from any entity in the system, there is no way to determine the balance of any of the privacy ledgers. 

<p align="center">
  <img src="https://github.com/yaksetig/zktoken/blob/main/figures/initial_balances.png" />
</p>

For simplicity, the reader can imagine the flow of transfer of balances on the commit chain and then a mint or burn locally to ensure that the internal state reflects the state of the commit chain. Failure to comply to these rules are effectively fraud and are caught via the use of zero-knowledge proofs that ensure that the internal state of the privacy ledger is the same as the balance on the commit chain. 


### Private Message Signaling
In our design, we assume that privacy ledgers communicate with each other via the central Commit Chain. This communication, however, must be encrypted to ensure that the data that is sent in the system is confidential. To encrypt the data we use an authenticated encryption with associated data (AEAD) scheme (i.e., AES-GCM-256). 

The problem with this trivial approach is that the recipient must have a mechanism to know that there are new messages for them. For example, if privacy ledger A wants to send an encrypted message to privacy ledger B, there must be a mechanism for A to publish a message on the commit chain without revealing that it is destined for B, thus keeping the privacy of the messaging component. 

To address this problem, we use a hash-based private message signaling tag that is included in the associated data of the ciphertext. The tag is calculated by hashing the latest block number along with the shared secret 's' between both parties. Therefore, t = Hash(block_number, s). As a result, the (encrypted) messages that are published on the commit chain have the following format: 

**t || Enc(k, m)**

In this case, t is the private message signaling tag, and Enc(k, m) represents the encryption of message "m" under (symmetric) key "k". 

To detect if there are messages for them, the relayers simply perform linear work 

We use the block number to ensure that the generation of these tags requires only an initial setup to obtain the shared secret. The relayers can then calculate these tags efficiently and see if there are any messages for them for that specific block. We note that this requires linear work per block as each relayer must calculate (N-1) tags. These tags, however, are very efficient to obtain and the work to generate these is parallelizable. 

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


