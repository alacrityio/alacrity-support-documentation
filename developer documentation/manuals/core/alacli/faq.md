# Clala FAQ

## Domain Socket (IPC) vs. HTTPS (RPC)

When connecting to `alacli` to `kalad`, there are two options. You have the option to either use domain sockets or HTTPS. Uses domain socket offering many benefits. It lowers the chance of leaking access of `kalad` to the LAN, WAN or internet. Also, unlike HTTPS protocol where many attack vectors exist such as CORS, a domain socket can only be used for the intended use case Inter-Processes Communication

## What does "transaction executed locally, but may not be confirmed by the network yet" means?

It means the transaction has successfully been accepted and finalized by the instance of alanode that `alacli` submitted it directly to. That instance of alanode should pass the transaction to additional instances via the peer-to-peer protocol but, there is no guarantee that these additional instances accepted or finalized the transaction. There also is no guarantee, at this point, that the transaction has been accepted and executed by a actual block producer and consequently included in a valid block in the blockchain. If you require stronger confirmation of a transactions inclusion in the immutable blockchain, you must take additonal steps to watch for the transactions presence in an unchangeable block.
