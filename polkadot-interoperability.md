---
description: Interoperability - scalability + security
---

# Polkadot - Interoperability

## **Why we rely on Polkadot for decentralized social network** 

After extensive research, we decided to build Woonkly using the Substrate development framework and, in the future, deploy Woonkly as a parachain on the Polkadot network, giving us 0 gas cost interoperability within the social network.  


### **Substrate Blockchain Framework** 

By building on top of this framework, we can take advantage of the extensive functionality that Substrate includes out of the box, rather than having to build it ourselves. This includes peer-to-peer networks, consensus mechanisms, governance functionality, an EVM implementation, and more.  
  
Overall, using Substrate will dramatically reduce the implementation time and effort required for our blockchain. Substrate allows a great deal of customization, which is necessary to achieve our Ethereum compatibility goals. And by using Rust, we benefit from both security guarantees and performance improvements.  
****

### **Polkadot network and ecosystem** 

The Polkadot net is also a good fit. As a parachain in Polkadot, we will be able to directly integrate and move tokens between any other parachain and parathreads on the network.  
  
We can also take advantage of any of the bridges that are built independently to connect non-Polkadot to Polkadot chains, including bridges to Ethereum. Polkadot's interoperability model uniquely supports cross-chain integration goals and is a key enabling technology.

![Testnet Woonkly in Polkadot running on Substrate - Rust](.gitbook/assets/image%20%2828%29.png)

### **We will use Moonbeam integrating EVM compatibility to the Polkadot framework** 

## **Technology** 

### **The Moonbeam development stack** 

Moonbeam is a smart contract blockchain platform built on the Rust programming language using the Substrate framework.  
  
**Rust programming language**  
  
Rust is a good language to implement a blockchain, as it is high-performance like C and C ++, but has built-in memory security features that are enforced at compile time, avoiding many common mistakes and security issues that can arise from C and C ++ implementations.  
  
**Substrate frame**

Substrate provides an extensive set of tools for building blockchains, including a runtime execution environment that enables a generic state transition feature and a set of pluggable modules that provide implementations of various blockchain subsystems.  
  
Moonbeam leverages multiple existing substrate framework palettes to provide key blockchain services and functionality, including core blockchain data structures, peer-to-peer networks, consensus mechanisms, accounts, assets, and balances. Custom palettes and runtime logic implement Moonbeam-specific behavior and functionality, such as cross-chain token integration. For leveraged pallets, Moonbeam will strive to stay as close to the upstream Substrate code base as possible and will continually incorporate Substrate bug fixes, enhancements and new features.

**Blockchain runtime**  
  
The main Moonbeam runtime specifies the state transition function and behavior of the Moonbeam blockchain. The Moonbeam runtime is built using [FRAME](https://docs.moonbeam.network/resources/glossary/#substrate-frame-pallets). It includes several standard pallets as well as several customized ones. The runtime is compiled into a [WebAssembly \(Wasm\)](https://docs.moonbeam.network/resources/glossary/#webassemblywasm) binary and a native binary. These compiled versions will run in the Polkadot Relay Chain and Moonbeam node environments  
  
**Technologically we are working in real decentralized interoperability  
  
Ethereum compatibility architecture**  
  
Smart contracts in Moonbeam can be implemented using Solidity, Vyper, and any other language that can compile smart contracts into EVM compliant bytecode. Moonbean aims to provide a safe, low-friction environment for smart contract development, testing, and execution that is compatible with the existing Ethereum developer toolchain.  
  
The execution behavior and semantics of Moonbeam-based smart contracts will strive to be as close to Ethereum Layer 1 as possible. Moonbeam is a single chunk, so cross-contract calls have the same synchronous execution semantics as in Ethereum Layer 1. 

![Diagram showing interactions possible thanks to Moonbeam&apos;s Ethereum support](.gitbook/assets/image%20%2858%29.png)

A high-level interaction flow is shown above. A Moonbeam node receives a Web3 RPC call from an existing DApp or Ethereum development tool, such as Truffle. The node will have both Web3 RPC and Substrate RPC available, which means you can use Ethereum or Substrate tools when interacting with a Moonbeam node. These RPC calls are handled by associated substrate runtime functions. The substrate runtime checks for signatures and handles extrinsic elements. The smart contract calls are finally passed to the EVM to execute the state transitions.  
  
By basing our EVM implementation on Substrate Pallet-EVM, we get a full Rust-based EVM implementation and support from the Parity engineering team.  


