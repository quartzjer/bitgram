# Bitgram - bitcoin native email, private and decentralized

> very rough draft, just brainstorming!

Overall concept is to cache encrypted messages (blobs) to recipients based in their public key along with a transaction, and when the recipient picks up the message from the cache they return a secret so that the cache can claim the transaction and get paid for the service.

## message delivery agent DHT

delivery agents advertise services via OP_RETURN
* per-byte-day rate
* ip:port
* hashname

## sending

* create a payload and secret-locked tx w/ payload hash and sealing key
* check agent DHT, validate 1+ agents/rates near the sealed hash, vary key to optimize
* create a secret-locked tx that is also 1-of-many p2sh to given agents
* fund the locked tx, include OP_RETURN of the sealed hash and recipient id
* send to nearby agents the payload and locked tx
* agents cache when verified

## receiving

* monitor blockchain for unread messages (the recipient id)
* contact agents
* request sealed from the unread tx
* get sealed copy, validate integrity, derive secret
* return secret to agent
* agent collects fee by broadcasting locked tx, which contains key to unseal
