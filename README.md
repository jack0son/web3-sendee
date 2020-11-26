# 💸 sendee

**Just send it** ✔️

_WIP - package in process of being extracted from dApp monorepo_

Web3 microservice with self healing subscriptions and transactions. Easily spin
up a web3 backend without worrying about transaction juggling, provider
failures, or subscription reliability. Built with [📬 Wact](https://github.com/jack0son/wact).

Run me as service, or start me from your dApp backend code. When I catch 🔥 it
will be at a safe distance.

- ✔️ Self healing subscriptions (events, blockheaders)
- ✔️ Intelligent transaction submission (gas calculation, nonce and address pool management)
- ✔️ Provider redundancy (provide a list of fall-through providers)
- ✔️ Isolate frequent web3 faults from the rest of your application
- ✔️ Built-in monitoring with sentry or twitter DMs

Sendee takes responsibility for blockchain nuances, applying its own policies for
problems like account balances, gas usage, and node availability, giving the
rest of the application a much smaller error and API surface to work
with, and some clean assumptions for how web3 interactions are handled.

**Why?**

When interacting with the Ethereum blockchain there is a broad set of failure
modes across transaction management, network connection, and event
subscriptions. Web3 itself has also been known for arbitrary failures caused by
bugs.

Sendee uses a recovery-oriented approach to masking failures, only reporting
back to the service caller when the fault is irresolvable or due to an invalid
parameter.

Allows you to separate the domain logic of your web3 service, such as an oracle,
market maker, analytics backend etc. from web3 boiler plate and failures so you
can focus more on behaviour and less on reliability.

It also implements useful preset algorithms for transaction preflight processes
like gas estimation, and nonce and address pool management, which can easily be
substituted or optimized for your use case.

- Cleanly decouple web3 provider management/supervision from blockchain interactions.
- Supervision policies with various levels of fault reporting granularity.
- Easily plug in your own supervision policies and preflight logic.

### How do I send a transaction?

## Under the hood

Sendee uses an actor model ([Wact](https://github.com/jack0son/wact)) to
address the recurring concurrency and reliability issues encountered with
complex web3 services, and isolate their failures into separate
execution contexts.

The core is a web3 provider actor as the gateway coordinator through which all other
components interact with the ethereum blockchain. The core ensures connection
availability and synchronises distribution of transaction nonces. Basically
a glorified semaphore.

1. Coordinate transaction nonces and provider connections
2. Isolate provider failures from transaction failures

Sendee gives you a number of fault tolerance and address.

Actors help seperate the frequent and broad set of errors that occur in web3
calls, from the simple core functionality we want: sending transactions and
subscribing to events.

For example:
* Provider / websocket connection errors
* Nonce errors
* Transaction errors (paramaters, client, node, eth network, onchain, etc)


One such assumption is that transactions will never fail due a lack of
connection to the ethereum node. Once the web3 service receives a transaction
message, it becomes responsible for confirming it with the network. Upon
failure, the requesting service receives a message which enacts it's
own policies for dealing with reverts and incorrect parameters.


## Fault Tolerance

### Provider Redundancy

### Subscription Keep-alive

## Transactions

Address pool / nonce management

- Transaction failures / retry
- Gas Policy
- Transaction cost management
- Transaction Replacement

## Contracts
