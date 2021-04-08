# CA in substrate
For consortium blockchain, CA is a important feature to authenticate the nodes and control the network. In substrate, the network is implemented in libp2p. It focus on the peer to peer communication, CA is more like a client/server centralized approach. So it is hard to implement the CA in libp2p stack according to research in libp2p codebase before.

## workaround
To use the CA in substrate, we have a workaround implementation. The solution is to exchange CA raw data after two substrate create the network connection via libp2p stack. The node will reject the peer if CA is failed in verification, or from different root CA organization. The implementation is checked in the branch of https://github.com/ParityAsia/substrate/tree/enterprise. Please check the readme in https://github.com/ParityAsia/substrate/tree/enterprise/scripts folder. Some CA files also in the same folder and can be used to test CA feature. You can follow the readme to create two substrate nodes with CA, then they can verify the peer and accept/reject connection according to the certificate.

## feature work
We need more elegant implementation for CA in substrate, that means more research in libp2p and figure out how to embed the CA exchange and verification in centain layer in libp2p stack.