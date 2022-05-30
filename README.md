# dBranch Node
The dBranch protocol allows journalists to publish articles via IPFS and use the cardano blockchain as a wire service to announce the existence of new articles.

*dBranch is currently in prototype phase.*

Overtime this protocol will become blockchain agnostic. The longterm goal is to use the immutable history of the blockchain to increase transparency and auditability, to create transparent versioning of articles and source information, and to use content addressing via IPFS to fix link rot in the citation process.

This repository is for the dbranch-node. It contains a docker file for [dbranch-backend](https://github.com/b-rad-c/dbranch-backend) and a compose file that creates all of the necessary services for running a dBranch node (currently only uses Cardano testnet). 

### dBranch ecosystem
Currently the dBranch software ecosystem consists of two user facing software apps and a node that contains all of the backend processes.

![dbranch software stack](/backend/doc/dbranch_node.jpg)


##### dbranch user software

[dbranch-desktop](https://github.com/b-rad-c/dbranch-desktop) - Trustless app allowing users to draft and publish articles directly into IPFS. In the future this will support discovery and browsing for articles across the dbranch protocol.
[dbranch-website](https://github.com/b-rad-c/dbranch-website) - A website for hosting published content. In the future this will be reusable so individual creators or news outlets can build their own websites. The current site is live at [dbranch.news](https://dbranch.news).

##### dbranch node

The [dbranch-node](https://github.com/b-rad-c/dbranch-node) repository (this one) contains the docker files that run the node, it consists of the [dbranch-backend](https://github.com/b-rad-c/dbranch-backend) and several 3rd party docker images.

##### dbranch backend
The [dbranch-backend](https://github.com/b-rad-c/dbranch-backend) repository is used to create two docker services:
* **dbranch-api-server** (backend for dbranch website)
* the **dbranch-curator** daemon which follows the blockchain history and curates *(copies and hosts via ipfs)* articles from configured wallets

##### 3rd party software/images
* **ipfs daemon:** host ipfs content
* **cardano-node:** official docker image to connect to cardano blockchain network
* **cardano-wallet:** used to sign articles and publish records to blockchain
* **cardano-db-sync:** indexes data to **postgres** from **cardano-node**
* **postgres:** used by **cardano-db-sync**

### Software Guide

![publishing via dbranch node](/backend/doc/dbranch_node_publishing.jpg)

##### steps to publish an article into the dbranch protocol
1. Using **dbranch-desktop** an author drafts an article and publishes it into IPFS
2. Taking the IPFS CID returned from step 1, the author signs the article using the **dbranch-curator** cli (future versions will include signing via **dbranch-desktop** UI). Signing an article means the author submits a transaction with metadata that includes the article CID and name, the transaction sends ADA from the author's wallet to their own wallet, effectively a moot transaction which serves simply to announce to the network that the article CID exists, creating an immutable publication history to create transparency and auditability.
3. The CLI connects to the *cardano-wallet* server which submits the transaction
4. The **cardano-node** receives the transaction request from the previous step and submits to the cardano mem pool for processing.

![dbranch node curator flow](/backend/doc/dbranch_node_curator.jpg)

1. The **dbranch-curator** runs continuously as a daemon and talks to the **postgres** instance to discover new articles published by configured cardano addresses.
2. The postgres instance is updated in the background by the official **cardano-db-sync** docker image
3. **cardano-db-sync** pulls its data from **cardano-node**
4. When new articles arrive they are published via **ipfs**

![dbranch user flows](/backend/doc/dbranch_node_user.jpg)
1. Users navigate to the **dbrach-website**, currently this only hosts content by the dbranch creators, will eventually be reusable so creators can host and/or curate their own content.
2. A future build will allow users to browse curators on the dbranch network using the **dbranch-desktop** application.
3. The **dbranch-api-server** gets its content from **ipfs**.