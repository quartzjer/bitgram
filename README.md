# Bitgram - bitcoin native email, private and decentralized

> very rough draft, just brainstorming!

Overall concept is to cache encrypted blobs to recipients based in their public key along with a transaction, and when the recipient picks up the message from the cache they return a secret so that the cache can claim the transaction and get paid for the service.

## message caching server pool

pool of agents advertise services via OP_RETURN
* per-byte-day rate
* ip:port, url
* track volume via payments?

## sending

* create a payload and secret-locked tx w/ payload hash and sealing key
* check market rates, and fund/broadcast the locked tx, include OP_RETURN of the sealed hash
* send to 1+ agents the payload and secret tx
* agents cache when verified

## receiving

* contact agents
* request sealed for an id
* get sealed copy and funding tx, validate integrity
* return secret to agent
* agent collects fee, output contains key to unseal
