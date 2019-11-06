# Taraxa Whitepaper - Building IoT’s Trust Anchor

version: 0.9.3

Disclaimer: this white paper is intended as a preliminary technical overview of the Taraxa protocol and ecosystem and is not meant to be comprehensive or fully finalized.


## Abstract

The advancement of IoT ecosystems has been consistently held back by technical challenges in security, maintenance, data provenance, and incompatible standards, as well as by business challenges in a generally lacking or lackluster use cases, strategically-risky platform lock-ins, and intentionally-siloed data management, and an increasing social awareness and demand for better privacy protection as devices become ubiquitous. Blockchain technology can help to address many of these challenges by building trust anchors into the ecosystem, thereby granting devices operational and economic independence, an awareness for asset ownership, and the capability to freely trade with one another. 
In this whitepaper, we introduce Taraxa, a public ledger focused on building trust anchors for IoT ecosystems with the following innovations, 

* Rapidly-finalizing DAG to maximize throughput and minimize inclusion & finalization latency
* Fuzzy sharding to minimize wasted work and maximize network-wide parallel processing 
* Speculative concurrency to minimize transaction execution latency
* Adaptive protocol to help the network learn and adjust its governing parameters on the fly
* Trustless light nodes that can verify what it’s been told by full nodes 


## Table of Contents



Demographic data often allows for re-identification [[1]](#1).



## 1. Introduction

### 1.1 The Promise of IoT

Long has the prospect of billions upon billions of intelligent, connected, and autonomous devices freely communicating and coordinating with one another been held up as an unquestioned vision for the future. The confluence and rapid advancement of technologies such as ever-more precise sensors and actuators, cloud computing, and near-ubiquitous connectivity have all made this vision seem increasingly likely. The market is full of audacious predictions of the world full of trillions of connected devices and the market for IoT reaching hundreds of trillions of dollars in market size. 

Yet, with each passing year, that promise has remained just that, a promise. 

For IoT to fully realize its promised potential, we would require a far more rapid pace in collecting and sharing of that collected data. IoT’s core value lies in the insights generated from the data collected by devices, which then in turn enable devices and humans to act upon these insights to generate value, eventually driving towards completely autonomous behaviors. As it stands today, the rate at which new data is being collected is slow, and the rate of sharing (and trading) of data is glacial. While by and large the sector has moved beyond the basic “what is IoT” understanding gap, it is nevertheless being held back by a set of very practical obstacles. 

**Technical obstacles**
Existing IoT infrastructures face a myriad of challenges as they scale up and eventually reaches trillions of autonomous devices. Centralized governance systems are ill-equipped to handle security as they present a single point of failure, they tend to scale poorly with workload as most device to device interactions tend to be localized, data collected by devices face questions of authenticity and provenance, and manufacturers in an attempt to create moats around their markets intentionally build in incompatible communication standards resulting in huge numbers of fragmented silos that take herculean costs and efforts to integrate. 

**Business obstacles** 
Due to the fundamental nascent nature of IoT systems as well as the various technical challenges, business cases for deploying large-scale IoT networks are often difficult to justify. The effort to discover financially-sound business cases are often hampered by data sensitivities tied to a fear of leaking trade know-how. Finally, with the rise of mega platforms and infrastructure players, businesses are becoming increasingly aware of the strategic risk of tying themselves into these centralized gardened walls, leaving them with a significantly-weakened bargaining position. 

**Social obstacles** 
With the rapid proliferation of digitized technologies, the public at large has become increasingly aware of the omnipresence of data-collecting sensors as well as concerned about how they’re being used. Recent data leaking scandals [1] [2] and the EU’s GDPR [3] have all placed privacy and data ownership at the center of civil discourse. If IoT as a technology is to continue proliferation, it must address data privacy concerns head-on and provide socially-acceptable solutions to guarantee secure data ownership and usage without triggering innovation-killing regulatory backlashes.

These and many more obstacles prevent the rapid adoption and value actualization of IoT in general. 


### 1.2 How Blockchain Technology Could Help 
Blockchain technology (for this section the term blockchain is used to refer generally to decentralized ledger technologies) has given us a tool for a decentralized set of participants to collaborate, trade, and compete without preestablished trust, radically diminishing or in many cases completely removing the need for centralized systems to provide guarantees and coordination. This decentralization has several interesting consequences as applicable to IoT. 

**Fractionalizing Resources**
Decentralization enables society at large to unlock underutilized assets such as storage, bandwidth, and computing power. Think of it like a shared economy without the centralized platform (shared riding without Uber, shared rooms without Airbnb), where idle resources could be more fully leveraged. This type of sharing also has the added effect of lowering the minimum purchase size for the individual or the small business owners, for whom in the past setting up an IoT infrastructure is prohibitively expensive. 

**Decentralized Security and Responsibility**
No longer are centralized coordination authorities necessary to facilitate transactions or maintenance updates. Blockchain enables fully decentralized networks (without single points of failure) that is far more secure and transparent. Hacking a single device wouldn’t get you access to a million IoT devices, just one, and the rest of the network hums along, significantly raising overall network security. Each device and device owner are now masters of their own device, its security, and the data it produces, making network maintenance a far more manageable task. 

**Open Source Ecosystem**   
One of the core features of public ledger ecosystems is that the underlying technology is all open-source, without which participants cannot be sure to trust the code that runs the ecosystem. This type of open-source ecosystem not only minimizes the risk of becoming platform-dependent (anyone can replicate the ecosystem at any given time if they choose to do so). For IoT data owners who may be reluctant to rely upon external platforms for mission-critical analytics, they can be sure that platform is fully decentralized, transparent, and free for anyone to duplicate, should they find any part of the ecosystem to their dissatisfaction. 

**Democratization of Data**
Blockchain can eventually enable devices to trade data with one another, enabling the much-heralded machine-to-machine economy. In time this economy will develop its own assets, pricing and reputations, each device its own business. With more sophisticated AI even generate its own algorithms to make use of the data available to maximize its own interests. By democratizing data, innovation (and business cases) would more readily and spontaneously blossom as ever more participants are granted access. 

Integrating blockchain technology into IoT would prove a significant boon to the space, enabling more data to be more freely shared and traded, triggering an explosive emergence of hitherto unthought-of applications. 


## 2. Review of Existing Projects

There are several blockchain projects purported to target the IoT space. Here we take a brief look at a few well-known projects. 

* IOTA uses a DAG-based Tangle instead of a linear ledger. It is miner-less and fee-less, achieved by having every node perform the tasks of verifying each other’s transactions. However, a finalized consensus protocol has yet to be published, currently relying on a closed-source, centralized coordinator to order and validate transactions. 

* HDAC is based on MultiChain, with a focus on mediating transactions and permissions between various public and private blockchains, with HDAC adding innovations such as an ePOW consensus in its private chain implementations. 

* VeChain is a DPoS-based chain, with permission-less participation but permissioned miner eligibility and governance model. Its applications are primarily focused on product lifecycle management. 

* IoTeX, adopts a chain of chains topology, enabling different IoT ecosystems to create their own sub-chains that can transact with one another through the root-chain, with a consensus algorithm similar to those proposed in Algorand and Dfinity (random selection of block proposal and validation committees). IoTeX also has functionalities to maintain transactional anonymity. 


Many important IoT use cases such as data anchoring are stateless transactions, whereby the key figure of merit for the blockchain network is simply block inclusion (see Section 4) instead of full finalization. Taraxa’s unique topology allows for a decoupling of inclusion as an asynchronous and independent network activity versus finalization & execution, allowing devices to get much faster feedback without having to wait for the remaining parts of the consensus. 
Device transactions also heavily depend on smart contracts. Taraxa combined the benefits of a fast-advancing DAG topology and the instant and fair finality of a VRF-driven PBFT process (see Section 4), giving smart contracts with state-dependent logic a definitive guarantee of immutability – not a probabilistic one. 
To support the role of smart contracts in device transactions, Taraxa’s unique use of speculative concurrency (see Section 5) in the construction of a concurrent VM drastically boosts execution speed, saving precious time for the entire network of full validating nodes. 
All networks evolve over time, and the best networks smoothly adapts to these evolutionary changes. Much of blockchain network’s evolution instead of happening automatically have taken place in online forums and offline meetings, often regressing into vicious and nonproductive disputes. Taraxa’s protocol is designed to seamlessly reach consensus on key network behavioral attributes such as block generation rate, block size, and shard jurisdictions, without all the fuss and drama. 


## References




## References
<a id="1">[1]</a> Rocher, L., Hendrickx, J. M., & De Montjoye, Y. A. (2019). Estimating the success of re-identifications in incomplete datasets using generative models. Nature communications, 10(1), 1-9.