FROM golang

# Swarm TCP; should be exposed to the public
EXPOSE 4001
# Swarm UDP; should be exposed to the public
EXPOSE 4001/udp
# Daemon API; must not be exposed publicly but to client services under you control
EXPOSE 5001
# Web Gateway; can be exposed publicly with a proxy, e.g. as https://ipfs.example.org
EXPOSE 8080
# Swarm Websockets; must be exposed publicly when the node is listening using the websocket transport (/ipX/.../tcp/8081/ws).
EXPOSE 8081

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

RUN ipfs bootstrap add /dnsaddr/ipfs.dbranch.news/p2p/12D3KooWEhBfgczYeJbKvdAmVYJSu3nT7dBSqBaj9HUYzxFr1hL9
RUN ipfs bootstrap add /ip4/138.68.200.23/tcp/4001/p2p/12D3KooWEhBfgczYeJbKvdAmVYJSu3nT7dBSqBaj9HUYzxFr1hL9
RUN ipfs bootstrap add /ip4/138.68.200.23/udp/4001/quic/p2p/12D3KooWEhBfgczYeJbKvdAmVYJSu3nT7dBSqBaj9HUYzxFr1hL9

RUN ipfs files mkdir --parents /dBranch/curated

COPY ./backend/samples/dbranch_intro.news /dbranch/
RUN ipfs add /dbranch/dbranch_intro.news

CMD ipfs daemon --enable-pubsub-experiment
