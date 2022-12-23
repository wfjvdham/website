---
title: Why do we Need a Mempool?
author: 'Wim van der Ham'
date: '2022-12-22'
slug: []
categories: ["bitcoin"]
tags: ["bitcoin"]
---

This post is a small summary of episode 50 of [bitcoin explained](https://podcasts.apple.com/us/podcast/death-to-the-mempool-long-live-the-mempool-episode-50/id1532957243?i=1000543194050) which is based on [this](https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2021-October/019572.html) tread on the bitcoin mailing list. The proposal is to remove the mempool from non-miner nodes and send transactions directly to miners. Here is an overview of why this is a bad idea.

# Why Would You Remove the Mempool?

- The current mempool requires resources from all nodes (both memory to store all the transactions in the mempool and bandwidth to relay transactions) which they provide in an altruistic way. While to create blocks, only miners have a financial incentive to have a mempool. This so they can construct the most profitable block.
- If you want to do a transaction you send it to everybody in the network while only a miner need to know it to be put inside a block. This is inefficient.
- Different nodes can have different mempools (for example as a result of different RBF settings) which increases complexity. 

# How to Remove the Mempool?

The proposed solution is to remove the mempool and send transactions directly to mining pools. These pools are open about there identity anyway and the number of pools is limited, so a list of tor endpoints can be published. Or a gossip mechanism can be used to distribute the mining endpoints. 

# Downside of not having a Mempool

- List of mining endpoints is a way of centralization, which gives power to the owner of the list. 
- If using a list of miner endpoints, the identity of miners need to be persisted which makes them vulnerable for DoS attacks. 
- Gossiping about the miner endpoints need some kind of mechanism to present a DoS attack. (transaction relaying is protected from this by the fee of the transactions)
- It becomes more difficult for smaller miners because wallets have an incentive to send a transaction just to the biggest miners with most of the hash power. This increases miner centralization.
- Sending to 1 miner is not enough because the miner will keep the transaction a secret for other miners so it can take it's fee.
- Sending blocks becomes bigger because all the transactions in the block need to be send as well. This is not the case when a node has (most) of these transactions already in it's mempool.
- Not everybody can validate what happens in the mempool, so censorship will be more difficult to discover.
- Miner can put high fee transactions in their own blocks to game the fee estimate. (this cannot be done when using the mempool because then there is a chance that another miner mines the high fee transaction)
- You cannot spend unconfirmed transactions anymore because you do not know about them.
- Privacy will actually be reduced because all transactions send from your node will be created by you. When you relay transactions the creator of the transactions is unknown.

So yes, better not remove the mempool :)
