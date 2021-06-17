---
description: >-
  For the transactions sent and added to a block, the Woonkly blockchain running
  on Hyperledger Besu does the validation of the transactions, as illustrated in
  the following diagram.
---

# Transaction validation

![Transaction validator](.gitbook/assets/image%20%2836%29.png)

The transaction pool validation set is repeated after the transaction is propagated.  
  
It also repeats the same set of validations when importing the block that includes the transaction, except that the nonce must be exactly correct when importing the block.  
  
When adding the transaction to a block, an additional validation is performed to verify that the gas limit of the transaction is less than the gas limit of the remaining block. After creating a block, the node imports the block and then repeats the transaction gas validations.  
  
**Important**  
  
The transaction is only added if the entire gas limit for the transaction is less than the remaining gas in the block. The total gas used by the transaction is not relevant for this validation. That is, if the total gas used by the transaction is less than the remaining gas in the block, but the gas limit in the transaction is greater than the remaining gas in the block, the transaction is not added.

