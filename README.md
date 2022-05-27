# dBranch Node
The repository is for the dbranch node. It contains a docker file for [dbranch backend](https://github.com/b-rad-c/dbranch-backend) and a compose file that creates all of the necessary services for running a dBranch node.

* dbranch api server
* dbranch curator daemon (follow blockchain history and publish articles from authorized wallets)
* ipfs daemon
* cardano-node
* cardano-wallet
* cardano-db-sync (indexes data to postgres from cardano-node )
* postgres 