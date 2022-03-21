# dBranch IPFS Node
### basic setup steps
* start with ubuntu 20
* create `ipfs` user & add to sudoers
* install go-ipfs
    - init repo
    - setup config with server profile
        - set `Addresses.Gateway` to `/ip4/0.0.0.0/tcp/8080`
        - set `Gateway.NoFetch` to `true`
* enable firewall
    - allow ssh traffic
    - allow tcp traffic to port 8080
* enable systemd service (as ipfs user)
    
        sudo mv dbranch-ipfs.service /etc/systemd/system
        sudo systemctl enable dbranch-ipfs

### to do
* api cors headers
* read reference article about ipfs specific headers

### references

* [ipfs as public gateway](https://rossbulat.medium.com/introduction-to-ipfs-set-up-nodes-on-your-network-with-http-gateways-10e21ea689a4)
* [ipfs configuration](ipns://docs.ipfs.io/how-to/configure-node/)
* [ubuntu firewall](https://www.howtogeek.com/115116/how-to-configure-ubuntus-built-in-firewall/)
