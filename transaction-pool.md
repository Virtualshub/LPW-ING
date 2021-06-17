---
description: >-
  All nodes maintain a pool of transactions to store pending transactions before
  processing.
---

# Transaction pool

**Options and methods for configuring and monitoring the transaction pool include**

  
****txpool\_besuTransactions JSON-RPC API method to list transactions in

the transaction pool.  
  
--tx-pool-max-size command line option to specify the maximum number

of transactions in the transaction pool.  
  
--tx-pool-hashes-max-size command line option to specify the number

maximum number of transaction hashes in the transaction pool.

It must be greater than or equal to the value specified for --tx-pool-max-size.  
  
****Command line option --tx-pool-price-bump to specify the percentage

price increase to replace an existing transaction.  
  
Command-line option --tx-pool-retention-hours to specify the number

maximum hours to keep pending transactions in the pool

of transactions.

newPendingTransactions and droppedPendingTransactions RPC subscriptions for

notify transactions added and withdrawn from the transaction pool.

**Important**  
  
When private transactions are submitted, the privacy marker transaction is sent to the transaction pool, not the private transaction itself.  
  
**Delete transactions when the transaction pool is full**  
  
When the transaction pool is full, it accepts and holds local transactions over remote ones. If the transaction pool is full of local transactions, Besu removes the oldest local transactions first. That is, a full transaction pool continues to accept new local transactions, removing remote transactions first and then older local transactions.  
  
**Substitution of transactions with the same sender and nonce**  
  
For transactions received with the same sender and nonce as a pending transaction but with a gas price higher than the existing one by the percentage specified by --tx-pool-price-bump, Besu replaces the pending transaction with the new one with the price of the higher gas. The default value of --tx-pool-price-bump is 10%.  
  
**Transaction pool size**  
  
Decreasing the maximum size of the transaction pool reduces memory usage. If the network is busy and there is a backlog of transactions, we can increase the size of the transaction pool and reduce the risk of removing transactions from the transaction pool.  


