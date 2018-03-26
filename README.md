- [WARNING](#warning)
- [iota-nelson-node with docker-compose](#iota-nelson-node-with-docker-compose)
  * [Getting Started](#getting-started)
    + [Prerequisites](#prerequisites)
  * [Usage](#usage)
    + [Clone the repository](#clone-the-repository)
      - [Change the Nelson config.ini](#change-the-nelson-configini)
      - [Change the Field config.ini](#change-the-field-configini)
    + [Start the node](#start-the-node)
    + [Check the logs](#check-the-logs)
    + [Open Nelson GUI](#open-nelson-gui)
    + [Open Nelson Monitor](#open-nelson-monitor)
    + [Open Grafana Dashboard](#open-grafana-dashboard)
    + [Update when a new release of any container is published](#update-when-a-new-release-of-any-container-is-published)
      - [Use the update script](#use-the-update-script)
      - [Update single container](#update-single-container)
      - [Update all containers](#update-all-containers)
  * [IRI Nodes information](#iri-nodes-information)
  * [Warnings](#warnings)
    + [Ports](#ports)
    + [Remote API limits](#remote-api-limits)
    + [Firewall (ufw) rules](#firewall--ufw--rules)
  * [More information](#more-information)
- [Author](#author)
- [License](#license)
- [Contributing](#contributing)
  * [Donations](#donations)
  * [TODO](#todo)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

# WARNING

I take no responsability about eventual damage! This project includes following alpha and beta software:
* CarrIOTA Nelson
* CarrIOTA Field
* Grafana 5

# iota-nelson-node with docker-compose

This repository contains the docker-compose file to get started with an IOTA/IRI node enhanced through the CarrIOTA project: Nelson.cli, Nelson.gui, Nelson.mon and Field.cli.
![top of dashboard](https://github.com/ioiobzit/iota-nelson-node/blob/master/images/nelson.png)

It also includes a Grafana 5 beta Dashboard enhanced through Prometheus, with information about:
* IRI node stats 
![iri stats](https://github.com/ioiobzit/iota-nelson-node/blob/master/images/iri.png)

* Server stats
![server stats](https://github.com/ioiobzit/iota-nelson-node/blob/master/images/server.png)

* [0mq](http://zeromq.org/) Metrics (local server)
![zmq stats](https://github.com/ioiobzit/iota-nelson-node/blob/master/images/zmq.png)

* Stresstest Stats (analytics.iotaledger.net)
![stresstest stats](https://github.com/ioiobzit/iota-nelson-node/blob/master/images/stresstest.png)

* Market Data in BTC/IOTA/EUR/USD
![market stats](https://github.com/ioiobzit/iota-nelson-node/blob/master/images/market.png)

* All neighbor stats
![all neighbor stats](https://github.com/ioiobzit/iota-nelson-node/blob/master/images/allneighbors.png)

* Stats by neighbors
![neighbor stats](https://github.com/ioiobzit/iota-nelson-node/blob/master/images/neighbors.png)

## Getting Started

These instructions will get you a copy of iri - the iota node with nelson up and running on your local machine using docker and docker-compose.

### Prerequisites

It is expected that you have already installed docker, docker-compose and know how to start and use it.
Knowledge about your operating system (Windows, Linux, MacOS).

## Usage

### Clone the repository
```
git clone https://github.com/ioiobzit/iota-nelson-node.git
```

#### Change the Nelson config.ini

Edit the `./volumes/nelson/config.ini` file to match your needs, for example the name, API username/password
```
[nelson]
name = CHANGE ME!
.
.
.
; Protect API with basic auth
[nelson.apiAuth]
username=user
password=pass
```

to
```
[nelson]
name = My awesome node
.
.
.; Protect API with basic auth
[nelson.apiAuth]
username=MyAweSomeUs3rnema
password=MyAweSomeP4ssw0rd
```

#### Change the Field config.ini

Edit the `./volumes/field/config.ini` file to match your needs, for example the name

```
[field]
name =  CHANGEME!! @antonionardella
```

to
```
[field]
name = My awesome node name
```

**Be sure to change your address field to your IOTA address for donations, otherwise thank you for leaving mine or add a new seed to get dynamically unused addresses *DO NOT USE YOUR MAIN WALLET SEED* **

Check your CarrIOTA Field node and donate to IOTA nodes here: http://field.carriota.com

### Start the node

Enter the iota-nelson-node folder
```
cd iota-nelson-node
```

Run it with:
```
docker-compose up -d
```
### Check the logs

Check the IRI logs with
```
docker logs iota
```

Check the Nelson logs with
```
docker logs nelson.cli
```

### Open Nelson GUI

Open your browser to
```
http://DockerHostIP:5000/#/<username>:<password>
```

### Open Nelson Monitor

Open your browser to
```
http://DockerHostIP:3000
```

### Open Grafana Dashboard

For the Grafana Dashboard to work, first we have to fix Prometheus. See [the documentation here](https://github.com/prometheus/prometheus/issues/2939).
Please go to `./volumes/prometheus` and execute the following command
```
sudo chown nobody. data
```

Restart the Prometheus container
```
docker-compose restart prometheus
```

Open your browser to
```
http://DockerHostIP:8000
```

Log in with:
```
Username: admin
Password: admin
```

and open the IOTA Dashboard

**PLEASE CHANGE YOUR ADMIN PASSWORD**

### Update when a new release of any container is published

#### Use the update script

This update script will pull all containers if updated or not and stop/remove/start **all cotainers**

Make the update script executable
```
cd iota-nelson-node
chmod +x update.sh
```

Run the update script
```
./update.sh
```

#### Update single container

Go to your iota-nelson-node folder and update the docker images
```
cd iota-nelson-node
docker-compose pull
docker-compose stop
docker-compose rm [container_name]
e.g. docker-compose rm nelson.cli
```

Run it with:
```
docker-compose up -d [container_name]
e.g. docker-compose up -d nelson.cli
```
#### Update all containers
Should docker give an error about aufs being busy stop all services and start them again.

e.g. nelson.cli and iota were updated after a snapshot:
```
cd iota-nelson-node
docker-compose pull
docker-compose stop
docker-compose rm iota
docker-compose rm nelson.cli
docker-compose up -d
```

## IRI Nodes information

The iota.ini contains three swarm nodes, this nodes will add you back automatically.

If you have other trusted nodes (e.g. you connected through [discord](https://discordapp.com/invite/fNGZXvh) or other trusted sources) be sure to adapt your `./volumes/iota/iota.ini` and `./volumes/nelson/config.ini` accordingly.
**Be aware that the ideal and maximum number of nodes so far is 7, no more, no less.**
```
Come-from-Beyond @here To ease the syncing issue reduce number of your neighbors. 7 should be the hard cap even if it's your mother asking to add her as the 8th neighbor. Use 3 neighbors if you are sure that they won't remove you without informing, use 5 if you are not sure in that. Thread in #nodesharingDec 4th at 9:54 AM
```
e.g. Your node is connected to **4** trusted IRI/IOTA nodes. The `NEIGHBORS` option in `./volumes/iota/iota.ini` will look something like this:
```
NEIGHBORS =  udp://host1:41041 tcp://host2:15600 udp://host3:14600 tcp://host4:15600
```

Then be sure to adapt the `outgoingMax` option in `./volumes/nelson/config.ini` to **3** to get a maximum of **7** nodes
```
outgoingMax = 3
```

As soon as the IRI/IOTA node is fully syncrhonized, please remove the swarm nodes `udp://88.99.249.250:41041 udp://94.156.128.15:14600 udp://185.181.8.149:14600` from your `./volumes/iota/iota.ini` and without stopping your node with curl:
```
curl http://DockerHostIP:14265 \
  -X POST \
  -H 'Content-Type: application/json' \
  -H 'X-IOTA-API-Version: 1' \
  -d '{"command": "removeNeighbors", "uris": ["udp://88.99.249.250:41041", "udp://94.156.128.15:14600", "udp://185.181.8.149:14600"]}'
```

and adapt your `./volumes/nelson/config.ini` accordingly to the number of trusted nodes  in the `./volumes/iota/iota.ini` config.

## Warnings

### Ports

The ports setup in the docker-compose.yml file opens following container ports

Port/Type | Use 
--- | ---
14265 | IOTA/IRI API port
14600/udp | IOTA/IRI UDP connection port
15600/tcp | IOTA/IRI TCP connection port
16600 | Nelson connection port
18600 | Nelson API port
21310 | CarrIOTA Field connection port
3000 | Nelson Monitor
5000 | Nelson GUI
9090 | Prometheus
9100 | Node Exporter
9311 | IOTA Prometheus Export as of [export default ports](https://github.com/prometheus/prometheus/wiki/Default-port-allocations)
8000 | Grafana Dashboard

Please assure yourself to set your firewall accordingly, the ports are opened on 0.0.0.0 (all IP adresses, internal and external)

### Remote API limits

**At this point NO API limits are now default!**

Following API limits are to be set as best practice (see iota.partners site or discussions on discord), but are not enabled as explained in the following table

parameter | explaination 
--- | ---
getNeighbors|No one can see the data of your neighbors
addNeighbors|No one can add neighbors to your node
removeNeighbors|No one can remove neighbors from your node
setApiRateLimit|This will prevent external connections from being able to use this command
interruptAttachingToTangle| To prevent users to do the PoW on your node
attachToTangle| To prevent users to do the PoW on your node

### Firewall (ufw) rules

The following rules have been used on my node, please adapt accordingly to your setup!
```
sudo ufw default allow outgoing
sudo ufw default deny incoming
sudo ufw allow ssh
sudo ufw allow 14265
sudo ufw allow 14600/udp
sudo ufw allow 15600/tcp
sudo ufw allow 16600
sudo ufw allow 18600
sudo ufw allow 21310/tcp
sudo ufw allow 8000
sudo ufw enable
sudo ufw limit 14265
sudo ufw enable
```
## More information

For more information about the combined projects please refer to the following github repositories:

* [IRI - IOTA Node](https://github.com/iotaledger/iri)
* [CarrIOTA Nelson client](https://github.com/SemkoDev/nelson.cli)
* [CarrIOTA Nelson GUI](https://github.com/SemkoDev/nelson.gui)
* [CarrIOTA Nelson monitor](https://github.com/SemkoDev/nelson.mon)
* [CarrIOTA Field client](https://github.com/SemkoDev/field.cli)
* [IOTA prometheus exporter](https://github.com/crholliday/iota-prom-exporter)

# Author

* **Antonio Nardella** - [Twitter](https://twitter.com/antonionardella) - info at antonionardella dot it

# License

This project is licensed under the ICS License - see the [LICENSE.md](LICENSE.md) file for details

# Contributing

## Donations

**Donations always welcome**:

IOTA:
```
CHQAYWPQUGQ9GANEWISFH99XBMTZAMHFFMPHWCLUZPFKJTFDFIJXFWCBISUTVGSNW9JI9QCOAHUHFUQC9SYVFXDQ9D
```

BTC:
```
1BFgqtMC2nfRxPRge5Db3gkYK7kDwWRF79
```

## TODO
- [x] [Update Grafana Deploy as soon as loading datasource from file is available in stable version](https://github.com/grafana/grafana/issues/5674) - https://github.com/grafana/grafana/pull/9504
- [x] [Update Grafana Deploy as soon as loading dashboards from file is available in stable version](https://github.com/grafana/grafana/pull/10052)
