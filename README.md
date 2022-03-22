# dBranch IPFS Node
### ipfs setup
* start with ubuntu 20
* create `ipfs` user & add to sudoers
* install [go-ipfs](ipns://docs.ipfs.io/install/command-line/#official-distributions)
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


### to do
* api cors headers
* read reference article about ipfs specific headers

### references

* [ipfs as public gateway](https://rossbulat.medium.com/introduction-to-ipfs-set-up-nodes-on-your-network-with-http-gateways-10e21ea689a4)
* [ipfs configuration](ipns://docs.ipfs.io/how-to/configure-node/)
* [ubuntu firewall](https://www.howtogeek.com/115116/how-to-configure-ubuntus-built-in-firewall/)
* nginx proxy for ipfs [one](https://rossbulat.medium.com/ipfs-with-react-loading-ipfs-assets-from-your-distributed-ipfs-gateways-fc601c8307bf) & [two](https://hackernoon.com/public-ipfs-node-behind-nginx-reverse-proxy-5682747f174b)
