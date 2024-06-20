# zktoken
Diagrams and code related to ZK Token

## System Architecture
Our architecture is simple, we assume a main EVM chain, which we call the "Commit Chain". Connected to each commit chain, there is a "Privacy Ledger". Each privacy ledger acts similarly to a validium L2. Therefore, each PL has a balance on the commit chain, but does not publish the transactions that take place internally. We note that, in our system, these balances are not public and are a feature of ZKT. 

<p align="center">
  <img src="https://github.com/yaksetig/zktoken/blob/main/zkt_architecture.png" />
</p>

## Protocols
The system is comprised of multiple protocols. 

### Setup
First, an admin must deploy the different contracts. 

### Registration
TBD

### Private Lookup
TBD


