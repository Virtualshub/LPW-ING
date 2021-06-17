---
description: 'Woonkly implements the following consensus protocols:'
---

# IFBT 2.0 consensus protocol

  
****

* **IBFT 2.0 \(Proof of Authority\)**

The config property in the genesis file specifies the consensus protocol for a string.

Proof of authority consensus protocols work when participants know each other and there is a level of trust between them. For example, in an authorized consortium network.  
  
The Proof of Authority consensus protocols have faster lock times and much higher transaction throughput than the Ethash Proof of Work consensus protocol used on the Ethereum mainnet.  
  
In Clique and IBFT 2.0, a group of network nodes act as signers \(Clique\) or validators \(IBFT 2.0\). Nodes in the signer / validator group vote to add or remove nodes from the group.

  
**Note**  
  
In the rest of this page, the term validator is used to refer to signers and validators.  
  
Properties The properties to take into account when comparing Clique and IBFT 2.0 are:  
  
Immediate purpose Minimum number of validators Responsiveness Speed. Immediate purpose IBFT 2.0 has immediate purpose. When using IBFT 2.0 there are no forks and all valid blocks are included in the main chain.  
  
Clique has no immediate purpose. Implementations using Clique must be aware of the forks and reorganizations of the chain that occur.  
  
Minimum number of validators To be Byzantine fault tolerant, IBFT 2.0 requires a minimum of four validators.  
  
Clique can operate with a single validator, but operating with a single validator does not offer redundancy if the validator fails.  
****

**IFBT**

Byzantine fault tolerance is the ability to function properly and reach consensus despite nodes failing or spreading incorrect information to peers.  
  
IBFT 2.0 networks require two-thirds of the validators to work to create blocks. For example, an IBFT 2.0 network of:  
  
****Four to five validators tolerate one non-responding validator. Six to eight validators tolerate two non-responding validators. Networks with three or fewer validators can produce blocks, but they do not guarantee purpose when operating in harsh environments.

