## Oracles and API3 (Price Feeds)

<!-- Hey Billy, 

I added your initial commit so I have a reminder/easy view of what the track plan is. Hope you don't mind.

re the current README.md: I'm doing quite a bit of guessing about the intentions with this file, so it's hard to give feedback with much confidence. Sorry to say.

Does it represent one lesson, or three from the eight you proposed? 

How many pages on Academy will this section/page need after you build it out/populate it more? One or three? 


The topics and sub-topics are really clear and nicely signposted, like I mentioned to you before, and the ideas to expand on are also clear.

Can you give me an idea of how much you 'will' expand on all the bullet points? 
E.g. 'Airnode' and 'First-party vs. Third-party Oracles' look like they
'might' be complete at this stage. But the most of the rest look like they are
intentions to populate. 

I was hoping to scaffold in some more around this and give more constructive feedback, but I'm stuck unless I have an idea about the above points. Can you share more of what's in your mind :) Cheers :)

Hopefully the following can help with a bit with communicating ideas with this scaffolding and for the learner 

It's a helpful 'concept' to create a meaningful flow for your track. I (and my colleagues) use this methodology if I'm planning a series of lessons. It's super adaptable. It might seem a bit pedantic, but it doesn't need to be rigid in language that you use. If you can envisage these below as parameters,you can always chop and change the values/arguments to suit your needs - just play musical chairs with them, if you feel a lesson is too long or two short: 

intro: build the context: where is the learner coming from => just got here and I assume you (learner) know about x, y  and z /what will learner do => a, b, c? 
< lesson > 
summary: what has learner just learned a, b, c < quiz > outro: what is their next step = d, e and f? 

repeat:
intro: build the context: where is the learner coming from => knows about a, b and c/what will learner do => d, e and f? 
< lesson > 
summary: what has learner just learned => d, e and f < quiz > outro: what is their next step = > g, h, and i? 

repeat:
intro: build the context: where is the learner coming from => knows about .... /what will learner do ...? 
< lesson > 
summary: what has learner just learned ... < quiz > outro: what is their next step ...?

repeat: etc

When we add in some warm-up quizzes it will also complement the intro in building context as well, by the way. It's better to get it in, and edit it out afterwards if it's repetitive or boring or whatever - or to give it a personal shake up for the better.

Also, it means if someone's doing a search, and they land in the middle of your track, they know there is a before lesson, and after lesson. But more importantly, when a learner comes back after a day or two, from one lesson to the next - let's face it, they've forgotten at least 50% of it - humans - we don't look like goldfish, but we share some of their traits. see https://en.wikipedia.org/wiki/Forgetting_curve


re: Shared workspace:

And it would be handier if we created a workbranch(es) on Academy v2, so we can use the Academy components - what say you? I can add in the (warm-up, check-point and final) quiz components for example - it might give you a better feel. I find this README.md is a messy way to communicate and exchange feedback to be really honest - all on the one plane. 

Ciao for now! -->

### About this lesson
In this course lesson, you will learn how oracles play a vital role in bringing offchain data to the blockchain.  The data we will be refering to in course is Price Feeds.

### Lesson Contents
1. Understanding the API Connectivity Problem
2. Key Components of API3
3. Breakdown of Decentralized APIs (dAPIs) - Price Feeds

### Understanding the API Connectivity Problem

#### Introduction
- **Objective**: To understand the challenge of integrating off-chain data with blockchain technologies, known as the API Connectivity Problem.
- **Background**: Explanation of the deterministic nature of blockchain and how it limits direct access to off-chain data.

#### The Problem Detailed
- **Smart Contracts' Limitation**: The EVM blockchain cannot access data outside of its own network natively.  It has no idea what the dollar value of what Ethereum is or any assets.  Being that the blockchain is also deterministic, there is no internal source of randomness.
- **Real-World Impact**: Scenarios where lack of external data access restricts the capabilities of blockchain applications. For instance, how a smart contract for weather-based crop insurance struggles without real-time weather data or how defi applications get the value of tokens.

#### API3's Solution
- **API3's Role**: API3 aims to bridge the gap by allowing decentralized access to off-chain data, enhancing blockchain applications' functionality permissionlessly.
- **Benefits**: Detailed analysis of the advantages of API3's approach, including increased security, reduced reliance on intermediaries, and enhanced data integrity.

### Key Components of API3

#### APIs in Blockchain

- **API3's Approach**: API3 facilitates a decentralized approach to integrate these APIs into blockchain environments, enhancing functionality and security using the Airnode infrastructure.

#### Airnode
- **Introduction to Airnode**: The Airnode is an architecture created by API3 to be the relay of data offchain bringing it onchain.  There are two types of oracles:  
- A push oracle, where the data is sent to the blockchain constantly via a certain criteria.  A price feed is a great example of a push oracle as we need to know the latest update of what the value of an asset is at any given time and need that onchain data immediately.
- A pull oracle, where the data is requested at the time of need and we wait for a reponse of that request.  A random number is a great example of how this would work.  We need one for a mint, send the mint request wait for the request and once the data is received do some logic.  This is a less time sensitive request.

![Airnode](./images/Airnode/Airnode%20image.png)


#### First-party vs. Third-party Oracles
- **Contrasting Oracles**: Many oracles send data on chain but how do we know the source in which the the data is coming from?  When getting a price feed for ETH, what is the source of price feed?  Coingecko, Coinbase, Coin Market Cap?  Most services, also combine (aggregate) the data to get the averaged value but how do we know the same date wasn't used more than once?  API3 uses a first party oracle system in wich the data source provider must host their own "Airnode" to provide the data onchain.  API3 does not act as a middle man in this feed and users can choose to read that single source.  API3 aggregates all these first party data feeds and provides that as a service and can reversabley be proven that the feed came from that specific source.

![1st Party](./images/Airnode/1st%20party.png)
![3rd Party](./images/Airnode/3rd%20party.png)



---

### Decentralized APIs (dAPIs) - Price Feeds

#### What are dAPIs?
- **Definition and Function**: Decentralized APIs (dAPIs), are essentialy price feeds.  They provide the connection from the offchain APIs for smart contract usage.

#### Functionality and Accessibility
- **Connecting Smart Contracts to Data Feeds**: In-depth discussion on how dAPIs enable smart contracts to access accurate and reliable real-world data feeds.
- **Real-World Example**: 
To read a dAPI value in a smart contract, import the IProxy.sol interface and call the read() function. Here's an example:

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.17;

import "@api3/contracts/v0.8/interfaces/IProxy.sol";

contract DataFeedReaderExample {
    // The proxy contract address obtained from the API3 Market UI.
    address public proxyAddress;

    // Set the price feed our contract wants to read
    function setProxyAddress(address _proxyAddress) public {
        proxyAddress = _proxyAddress;
    }

    function readDataFeed()
        external
        view
        returns (int224 value, uint256 timestamp)
    {
        // Use the IProxy interface to read a dAPI via its
        // proxy contract .
        (value, timestamp) = IProxy(proxyAddress).read();
        // If you have any assumptions about `value` and `timestamp`,
        // make sure to validate them after reading from the proxy.
    }
}
```
You'll have to set the proxyAddress using the setProxyAddress() function. You can get the proxyAddress for activated dAPIs from within the data feed dashboard through the [API3 Market](https://market.api3.org/dapis)


---
![MarketPlace](./images/dAPI/market.gif)

---

Once you have set your contracts proxy address, you contract is ready to read the external price feed and be usable for whatever logic you want to use that price data for.

#### Practical Application
- **What the Contract Returns**: When you click on readDataFeed it will return 2 individual numbers.
1. The Current Price of the Asset in 18 decimal format
2. The block.timestamp of when the price feed was last updated. (To ensure we don't have a stale price feed)

The function `readDataFeed()` will return two values.  The first value being the price of the asset in 18 decimals. The second value is the timestamp of when the price was last updated.  This is extremely important in production because we want to make sure that our price feeds are active and not stale.
API3 updates price feeds under two conditions, a change in price (different deviations 1%, 0.5% or 0.25%) or if the price hasn't change that much in 24 hours it will update just to make sure the price feed is active (think of a stable coin like USDC)

Once you have your data, you can add your logic to your smart contract such as setting a consistent price for an NFT or donation or a borrow lending dapp.

Looking for framework examples?

- [Foundry Example](https://github.com/api3-ecosystem/data-feed-reader-example-foundry)

- [Hardhat Example](https://github.com/api3dao/data-feed-reader-example)
