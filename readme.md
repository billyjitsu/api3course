## Oracles and API3 (Price Feeds and Random Numbers)

### About this lesson
In this course lesson, you will learn how oracles play a vital role in bringing offchain data to the blockchain.  The data we will be refering to in course is Price Feeds and Random numbers.

### Lesson Contents
1. Understanding the API Connectivity Problem
2. Key Components of API3
3. Breakdown of Decentralized APIs (dAPIs)
4. Understanding Quantum Random Number Generation (QRNG) in API3
5. Introduction to First-Party Oracles
6. Exploring Airnode's Self-Operating Feature
7. Additional Features of Airnode
8. How Airnode Works

### Lesson 1: Understanding the API Connectivity Problem

#### Introduction
- **Objective**: To understand the challenge of integrating off-chain data with blockchain technologies, known as the API Connectivity Problem.
- **Background**: Explanation of the deterministic nature of blockchain and how it limits direct access to off-chain data.

#### The Problem Detailed
- **Smart Contracts' Limitation**: The EVM blockchain cannot access data outside of its own network natively.  It has no idea what the dollar value of what Ethereum is or any assets.  Being that the blockchain is also deterministic, there is no internal source of randomness.
- **Real-World Impact**: Scenarios where lack of external data access restricts the capabilities of blockchain applications. For instance, how a smart contract for weather-based crop insurance struggles without real-time weather data or how defi applications get the value of tokens.

#### API3's Solution
- **API3's Role**: API3 aims to bridge the gap by allowing decentralized access to off-chain data, enhancing blockchain applications' functionality permissionlessly.
- **Benefits**: Detailed analysis of the advantages of API3's approach, including increased security, reduced reliance on intermediaries, and enhanced data integrity.

### Lesson 2: Key Components of API3

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

### Lesson 3: Decentralized APIs (dAPIs) - Price Feeds

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
Add gif for proxy contract
---

#### Practical Application
- **What the Contract Returns**: When you click on readDataFeed it will return 2 individual numbers.
1. The Current Price of the Asset in 18 decimal format
2. The block.timestamp of when the price feed was last updated. (To ensure we don't have a stale price feed)

### Lesson 4: Understanding Quantum Random Number Generation (QRNG) in API3

#### Introduction to QRNG
- **The Random Number Issue**: Currently, blockchains are mathematically deterministic.  Tricks like block.timestamp and adding a varitant can be calculated by a validator and gamed to get the result they want ruining the fairness of the idea.
- **API3's Approach to QRNG**: API3 solves this issue by using a Request/Response approach in a two transaction system.

#### The Role of ANU Quantum Random Numbers API
- **Collaboration with ANU**: Detailed explanation of API3's collaboration with the Australian National University, focusing on how the ANU Quantum Random Numbers API contributes to QRNG services.
- **Technical Breakdown**: A deeper look into the quantum mechanics involved in the generation of random numbers, explaining how this technology is practically applied in the blockchain context.

#### Real-World Implications
- **Use Cases**: Discussion on various use cases of QRNG in blockchain, such as in gaming, financial services, or any application requiring high levels of data unpredictability and security.

### Lesson 5: Introduction to First-Party Oracles

#### Defining First-Party Oracles
- **Explanation**: Detailed discussion on what first-party oracles are, their significance in blockchain technology, and how they differ from traditional oracles.
- **API3's Implementation**: Exploration of how API3 leverages first-party oracles, emphasizing direct data provision from the source to the blockchain, and the benefits of this model over traditional methods.

#### Advantages of First-Party Oracles
- **Security**: In-depth analysis of how first-party oracles enhance data security by eliminating intermediaries and reducing points of failure.
- **Cost Efficiency and Transparency**: Examination of how first-party oracles reduce operational costs and increase transparency in data transactions.
- **Flexibility**: Discussion on the adaptability of first-party oracles in various blockchain applications, supported by real-world examples.

#### Governance in First-Party Oracles
- **Decentralized Governance**: Detailed explanation of the decentralized governance model in first-party oracles, focusing on how it ensures trustlessness and security in data provision.
- **Real-World Implication**: Case studies demonstrating the effectiveness of decentralized governance in maintaining data integrity and reliability.


### Lesson 6: Exploring Airnode's Self-Operating Feature

#### Airnode's Autonomous Operation
- **Self-Operating Nature**: Detailed exploration of Airnode’s self-operating feature, explaining how it allows API providers to autonomously run their own oracle nodes without intensive management or technical expertise.
- **Benefits for API Providers**: In-depth discussion on how this feature simplifies the process for API providers, reducing the need for constant monitoring and maintenance.

#### Practical Benefits
- **Reduced Overhead**: Analysis of how Airnode’s self-operating nature leads to reduced operational overhead for API providers, with examples illustrating cost savings and efficiency gains.
- **Case Example**: A real-life case study of an API provider using Airnode, showcasing the ease of setup, operation, and the impact on their blockchain service offering.

### Lesson 7: Additional Features of Airnode

#### Advanced Functionalities
- **Feature Overview**: Comprehensive review of Airnode's additional features such as pre and post-processing, authentication, and authorizations, explaining each feature in detail and its importance.
- **Enhancing dApp Functionality**: Discussion on how these advanced features can be leveraged in different blockchain applications to enhance functionality, security, and user experience.

#### Use Case Scenarios
- **Example Applications**: Presentation of several use case scenarios demonstrating how Airnode’s additional features can be utilized in blockchain applications, with examples ranging from finance to supply chain management.

### Lesson 8: How Airnode Works

#### Airnode's Mechanism
- **In-Depth Explanation**: Detailed description of the operational mechanism of Airnode, covering how it retrieves data from off-chain sources and provides it on-chain.
- **Technical Aspects**: Exploration of the technical features that facilitate Airnode’s operation, such as its stateless nature and serverless architecture, discussing the benefits and efficiencies these features provide.

#### Security in Airnode
- **Defensive Design Philosophy**: A thorough examination of the security measures and design philosophies behind Airnode, detailing how these contribute to the overall security and reliability of the oracle node.
- **Real-World Example**: A case study illustrating Airnode’s security design in action, focusing on a specific blockchain application where Airnode’s security features played a crucial role.

---

This course content provides an extensive and detailed understanding of API3 and Airnode, covering technical aspects, practical applications, and real-world examples. Each lesson is carefully crafted to provide comprehensive knowledge, supplemented with case studies and use case scenarios to contextualize the concepts in practical settings. The course aims to equip learners with a deep understanding of these pivotal blockchain technologies, preparing them for practical application and further exploration in the field.
