# How to create an UniSwap Clone DEX on Polygon
UniSwap is an Automated Market Decentralized Exchange. UniSwap makes it possible for users on the Blockchain to SWAP from one token to another at a fee. The cost of this SWAP is usually expensive on the Ethereum network. In this tutorial, we fork the UniSwap contracts and build a clone using ReactJS. We will deploy the DApp on Polygon and this will enable us to spend a small amount on Swap transactions.

# Prerequisites
In this tutorial, we are going to build a token SWAP DApp by cloning the UniSwap contracts and we will deploy the DApp on Polygon.

It is recommended that you complete the Figment [Learn Polygon](https://learn.figment.io/pathways/polygon-pathway) Pathway.

# Requirements

This article assumes that you have prior knowledge of ReactJS. Also, that you have installed:
[Node v12+](https://nodejs.org/en/download/)
[MetaMask](https://metamask.io/).
[Ganache](https://www.trufflesuite.com/ganache)


# Content

# Basic introduction to traditional market makers and Automated Market Makers. 
In traditional securities market, we have entities referred to as: “Market Makers”. Market Makers provide trading services for investors in a particular securities market, enabling liquidity. Market Makers can be individuals or organizations. What a market makers does is: 
“They provide quotes by stating BUYS and SELL options on a pair of security commodities.”

In financial terms, they provide: LIQUIDITY.

Liquidity is the amount of a securities available in a market, thus making it possible to buy and sell (trade) such a pair. A market with two token pairs can have an amount of the token pair making it possible for traders to buy and sell without the total number of one or both tokens being exceeded in sales. 

“Liquidity involves the tradeoff between the price at which an asset can be sold, and how quickly it can be sold.” - Investopedia

A market maker can offer an asset at a BUY of $10 and a SELL of $10.05. In simple terms, the market maker is willing to buy a commodity at $10 and sell that same commodity at $10.05. Thus making a $.05 profit on the said commodity. This enables Market Makers to provide liquidity to the markets in which they operate and make profits from the BUY-SELL positions. They also take on the risks of loss.

## What is market liquidity and why is it important. 
“In business, economics or investments, market liquidity is a market’s feature whereby an individual or firm can quickly buy or sell an asset without causing a drastic change in the asset price.” - Investopedia

## Automated Market Makers
In very simple terms, an Automated Market Maker, AMM removes the need for intermediaries to enable BUY - SELL orders. An AMM relies on the use of Smart Contracts to carry out market activities on the Blockchain. A central aspect of an AMM is the ability to SWAP from one token to another using a Decentralized Exchange DEX.

This article focuses on understanding the SWAP mechanism of a DEX and how to build one and launch on Polygon chain. To achieve this we are going to clone the UniSwap Smart Contracts and build a UI using ReactJS. 

First, it is important to understand the following things under the hood:
How a SWAP is carried out.
The importance of Liquidity when carry out SWAP.
How to calculate available liquidity for token pairs using the XY=K formula.
