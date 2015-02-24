# Bitgram - bitcoin distributed compute

> very rough draft, just brainstorming!

* use blockname DHT to advertise compute service(s)
* select 1+ providers
* choose based on price, reputation, trust
* upload a container
  * during compute, containers require checkpoint sync
* fund
  * negotiate penny banks to fund a distributed compute
  * accessor pays
  * timelocked
  * container has penny bank, pays for self

Overall concept is to cache encrypted messages (blobs) to recipients based in their public key along with a transaction to a set of random delivery agents, and when the recipient picks up the message from any agent they return a secret so that the agent can claim the transaction and get paid for the service.

## message delivery agent DHT

Active delivery agents join a [blockname](https://github.com/telehash/blockname) DHT of `.bitgram`, resolution always returns the closest active agents to a given query and their current per-byte-day rate.

## sending

* create an encrypted payload and nonce, hash is key
* contact 1+ agents on the DHT, vary nonce to optimize location
* perform zero-knowledge negotiation (like [penny bank](http://quartzjer.github.io/pennybank/)) to select secret from each agent
* end up w/ hash of a secret for the frozen tx, and hash of the sealed payload per-agent
* frozen tx is 1-of-many p2sh of all agents
* build [merkle tree](http://en.wikipedia.org/wiki/Merkle_tree) of the sealed hashes, use top hash in OP_RETURN of tx funding the p2sh of the frozen tx so recipient can validate
* distribute frozen tx and all sealed hashes to all agents
* create a payload and secret-locked tx w/ payload hash and sealing key
* agents cache when verified

## receiving

* monitor blockchain for unread messages? (the recipient id)
* contact agents based on the DHT/key
* request sealed from the unread tx
* get sealed copy and section of merkle tree to validate integrity
* derive secret, return to agent
* agent collects fee by broadcasting frozen tx w/ secret, always contains key to unseal

## compute

> wild west

* select 1+ compute nodes from the DHT
* negotiate escro'd microtx (penny bank) with all
* share the "difference" of secrets w/ the nodes (each get others but not own)
* nodes must all collaborate to "compute", carefully share secrets to get their own
* program signals sync events w/ dynamic state, used to validate all nodes
* all nodes must have all secrets and sign final tx
* may only be triggered by blockchain events (getting funded?)
