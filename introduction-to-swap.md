---
description: Buy and Sell Woops in a decentralized way
---

# Introduction to swap

  
At Woonkly.com you can buy and sell our "WOOP" token created on the Binance Smart Chain Network \(Bep20\) in a decentralized way. Users can provide liquidity to the Pool and thereby obtain a percentage of the profits generated in the Swap.  
  
****This technology is also used in our DEX ERC20 / BEP20 that will see the light on May 1, 2021 and in our DEX Crosschain ERC20 / BEP20, unique in the world, which will allow users to exchange any token of the ERC20 network with any token of the BEP20 network and vice versa \(the Dex Crosschain is pending regulation\).

\* All our products and / or services will be oriented to comply with the regulations imposed by the European Union  


## **Decentralized constant liquidity exchange**

![](.gitbook/assets/image%20%2866%29.png)

## **Why is the DEX automated?** 

Why automated? Woonkly uses an automated market maker instead of a more traditional microstructure for several reasons:  
****

**Solves the buyer / seller pairing problem:** Automated market maker ensures that the market is always available to trade, even at two in the morning in your local time zone, at the start of the market, and even if there are no other active traders at that moment. By giving traders the confidence that they can get liquidity for their bets whenever they want, automated market makers create additional incentives to participate.  


**Enable large markets with many events:** Consider a prediction market for which US presidential candidate will win the election. Possibly, there could be dozens of different actively traded candidates, subject to the restriction that their cumulative odds cannot add up to more than 100%. These large event spaces are very difficult for human participants to seed, requiring careful and precise probability estimates for very small values. Consequently, existing human-mediated prediction markets with a large number of results tend to have very wide bid/ ask margins, especially for the darkest candidates. Automated market makers mitigate this problem by behaving consistently in markets with large or small numbers of results.  


**Concisely expresses the current state of the market:** One of the concerns in a distributed system like Augur is that each part of the shared state increases the possibility of inconsistencies between the opinions of various participants and increases the amount of time for different views of the market line up. Automated Market Makers can be integrated and distributed with a minimal amount of status. This can be contrasted with a traditional double auction in which the full market status includes the entire order book and where the operations may include participants withdrawing or modifying their existing orders.  
****

**It allows precise control of losses in the worst case scenario by the market liquidity contributors:** The initiator of a market assumes the risk of losing a certain amount \(which can always increase\) in exchange for a variable payment derived from commercial rates. As the amount risked by the sponsor increases, the market deepens and individual stocks cause less price slippage. An interesting result of this cap is that markets can enter unconditional profit states for their backers, where an active market has collected enough trading fees to make the market profitable for its backer regardless of future bets and regardless of the result that actually takes place.  


**Automated Market Makers Avoid Common Trader Mistakes and Information Redundancy:** Research studies have shown that Iowa Electronic Marketplaces \(IEM\) merchants often violate the "law of one price." Consider betting on which of the two candidates will win. Logically, a bet that candidate A wins the election is equivalent to a bet that candidate B wins. However, in traditional double auctions, the prices of these contracts can diverge and, in addition, the IEM administrators observed that many traders take positions in the worst irrational contract. In contrast, automated market makers enforce the equality of logically equivalent bets.  
****

**Woonkly DEX is an Automated Market Maker \(AMM\).** You can think of an AMM as a primitive robotic market maker who is always willing to quote prices between two assets according to a simple pricing algorithm. Woonkly DEX sets the price of the two assets so that the number of units it has of each asset, multiplied, is always equal to a fixed constant. That's a bit tricky: if Woonkly DEX owns some units of token x and some units of token y, it sets the price of any trade so that the final amounts of x and y it owns, multiplied together, are equal to a fixed constant, k. This is formalized as the constant product equation: x \* y = k.  


![Constant Liquidity Matrix](.gitbook/assets/image%20%2860%29.png)

