FROM golang

WORKDIR /dbranch
RUN wget https://dist.ipfs.io/go-ipfs/v0.12.2/go-ipfs_v0.12.2_linux-amd64.tar.gz
RUN tar -xvzf go-ipfs_v0.12.2_linux-amd64.tar.gz

WORKDIR /dbranch/go-ipfs
RUN sh install.sh

WORKDIR /dbranch
RUN ipfs init
RUN ipfs config Addresses.Gateway /ip4/0.0.0.0/tcp/8080
RUN ipfs config Addresses.API /ip4/0.0.0.0/tcp/5001

RUN ipfs config --json API.HTTPHeaders.Access-Control-Allow-Origin "[\"*\"]"
RUN ipfs config --json API.HTTPHeaders.Access-Control-Allow-Methods "[\"PUT\", \"GET\", \"POST\"]"

CMD ipfs daemon --enable-pubsub-experiment
