# dBranch IPFS Node
The repository for the dbranch node. Long term plan is to run an ipfs node and other blockchains such as Cardano and/or Algorand in addition to the [dbranch backend](https://github.com/b-rad-c/dbranch-backend) and stuff it all into a docker compose file.


### ipfs setup
- start with ubuntu 20
- create `ipfs` user & add to sudoers

        adduser ipfs
        usermod -a -G sudo ipfs

- install [go-ipfs](ipns://docs.ipfs.io/install/command-line/#official-distributions)
    - init repo
    - setup config with server profile
        - set `Addresses.Gateway` to `/ip4/0.0.0.0/tcp/8080`
        - set `Gateway.NoFetch` to `true`
        - add CORS support on `API.HTTPHeaders` and `Gateway.HTTPHeaders`

                "Access-Control-Allow-Methods": [
                        "PUT",
                        "GET",
                        "POST",
                        "OPTIONS"
                ],
                "Access-Control-Allow-Origin": [
                        "*"     // or other specific address
                ]

* enable firewall

        sudo ufw allow ssh
        sudo ufw allow 8080/tcp
        sudo ufw enable


* enable systemd service (as ipfs user)
    
        sudo mv dbranch-ipfs.service /etc/systemd/system
        sudo systemctl enable dbranch-ipfs

### dbranch backend

        wget https://go.dev/dl/go1.18.linux-amd64.tar.gz
        sudo tar -C /usr/local -xzf go1.18.linux-amd64.tar.gz
        rm go1.18.linux-amd64.tar.gz
        echo 'export PATH=$PATH:/usr/local/go/bin' >> /etc/profile
        echo 'export PATH=$PATH:/usr/local/go/bin' >> $HOME/.bashrc
        git clone https://github.com/b-rad-c/dbranch-backend.git
        cd dbranch-backend
        go build main.go
        sudo mv main /usr/local/bin/dbranch-backend
        

### nginx setup

    sudo apt update
    sudo apt install -y nginx
    sudo ufw allow 'Nginx Full'
    sudo rm /etc/nginx/sites-enabled/default
    sudo rm /etc/nginx/sites-available/default

    # be sure to change the server_name line in the following conf file to your ip address or hostname
    sudo mv nginx-ipfs-proxy.conf /etc/nginx/sites-available/nginx-ipfs-proxy.conf
    sudo ln -s /etc/nginx/sites-available/nginx-ipfs-proxy.conf /etc/nginx/sites-enabled/nginx-ipfs-proxy.conf
    sudo service nginx restart

### certbot set up
Enable auto renewing SSL cert with `certbox` for nginx on ubuntu 20 at [with instructions at this link.](https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal)


### references

* [ipfs as public gateway](https://rossbulat.medium.com/introduction-to-ipfs-set-up-nodes-on-your-network-with-http-gateways-10e21ea689a4)
* [ipfs configuration](ipns://docs.ipfs.io/how-to/configure-node/)
* [ubuntu firewall](https://www.howtogeek.com/115116/how-to-configure-ubuntus-built-in-firewall/)
* nginx proxy for ipfs [one](https://rossbulat.medium.com/ipfs-with-react-loading-ipfs-assets-from-your-distributed-ipfs-gateways-fc601c8307bf) & [two](https://hackernoon.com/public-ipfs-node-behind-nginx-reverse-proxy-5682747f174b)
