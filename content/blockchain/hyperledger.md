---
title: "Hyperledger"
date: 2017-11-13T11:32:26+01:00
draft: false 
---

### tmp

https://www.hyperledger.org/
https://www.hyperledger.org/blog/2017/11/29/build-it-on-blockchain-a-sustainable-palm-oil-industry

http://hyperledger-fabric.readthedocs.io/en/release/getting_started.html
https://www.ibm.com/developerworks/cloud/library/cl-ibm-blockchain-101-quick-start-guide-for-developers-bluemix-trs/index.html


https://unix.stackexchange.com/questions/363048/unable-to-locate-package-docker-ce-on-a-64bit-ubuntu

```
sudo apt-get update
sudo apt-get install docker-ce
sudo docker run hello-world
sudo curl -L https://github.com/docker/compose/releases/download/1.17.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
sudo apt-get install emacs24 emacs24-el emacs24-common-non-dfsg
```

```
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
sudo apt -y install npm
sudo apt-get install python
python --version
git config --global core.autocrlf false
git config --global core.longpaths true
git config --get core.autocrlf
git config --get core.longpaths
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get install -y nodejs
```

```
git clone -b master https://github.com/hyperledger/fabric-samples.git
cd fabric-samples
```

http://hyperledger-fabric.readthedocs.io/en/latest/write_first_app.html
https://github.com/hyperledger/composer/blob/master/contrib-notes/getting-started.md

```
git clone https://github.com/hyperledger/composer.git
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.6/install.sh | bash
npm install --global lerna

cd composer
npm run bootstrap

npm start

ssh -L 9000:localhost:3000 ubuntu@10.15.24.231

```

start composer
./composer.sh
ssh -L 8080:localhost:8080 ubuntu@10.15.24.231


```
ssh -L 9000:localhost:3000 ubuntu@10.15.24.231
```
Could not detect web browser to use - please launch Composer Playground URL using your chosen browser ie: <browser executable name> http://localhost:8080 or set your BROWSER variable to the browser launcher in your PATH

--------------------------------------------------------------------------------------
Hyperledger Fabric and Hyperledger Composer installed, and Composer Playground launched
Please use 'composer.sh' to re-start, and 'composer.sh stop' to shutdown all the Fabric and Composer docker images



try to install stable release
https://github.com/hyperledger/composer/releases
https://hyperledger.github.io/composer/installing/development-tools.html

```
wget https://github.com/hyperledger/composer/archive/v0.16.0.tar.gz
tar xvf v0.16.0.tar
rm v0.16.0.tar
cd composer-0.16.0/

sudo npm install --global yo --allow-root
```

```
composer network start --card PeerAdmin@hlfv1 --networkAdmin admin --networkAdminEnrollSecret adminpw --archiveFile tutorial-network@0.0.1.bna --file networkadmin.card
```

```
```

https://hyperledger.github.io/composer/integrating/getting-started-rest-api.html

### IOT

https://www.ibm.com/developerworks/cloud/library/cl-deploy-interact-extend-local-blockchain-network-with-hyperledger-composer/index.html

```
npm install -g composer-rest-server
composer network deploy -a dist/iot-perishable-network-advanced.bna -A admin -S adminpw -c PeerAdmin@hlfv1 -f networkadmin.card

cd fabric-tools/
./downloadFabric.sh 
./startFabric.sh 
./createPeerAdminCard.sh
cd developerWorks/
cd iot-perishable-network-advanced
composer card list --name PeerAdmin@hlfv1 
composer network deploy -a dist/iot-perishable-network-advanced.bna -A admin -S adminpw -c PeerAdmin@hlfv1 -f networkadmin.card
composer card import --file networkadmin.card

composer card list
composer network ping --card admin@iot-perishable-network-advanced


composer-rest-server

ssh -L 9999:localhost:3000 ubuntu@10.15.24.231
```

```
composer transaction submit --card admin@iot-perishable-network-advanced -d '{"$class": "org.acme.shipping.perishable.SetupDemo"}'

```


```
composer transaction submit --card admin@iot-perishable-network-advanced -d '{ "$class": "org.acme.shipping.perishable.TemperatureReading", "centigrade": 0, "shipment": "resource:org.acme.shipping.perishable.Shipment#SHIP_001"}'

composer transaction submit --card admin@iot-perishable-network-advanced -d '{ "$class": "org.acme.shipping.perishable.ShipmentReceived", "shipment": "resource:org.acme.shipping.perishable.Shipment#SHIP_001"}'


# composer identity issue --card admin@iot-perishable-network-advanced --file ID_CARD_FILE --newUserId IDENTITY --participantId 'resource:org.acme.shipping.perishable.PARTICIPANT#PARTICIPANT_ID'
```

# composer transaction submit --card admin@iot-perishable-network-advanced -d '{}' 'http://localhost:9999/api/org.acme.shipping.perishable.SetupDemo'


#### issue user and import card


```
composer identity issue --card admin@iot-perishable-network-advanced --file grower1.card --newUserId grower1 --participantId 'resource:org.acme.shipping.perishable.Grower#farmer@email.com'

composer card import --file grower1.card

composer identity issue --card admin@iot-perishable-network-advanced --file shipper1.card --newUserId shipper1 --participantId 'resource:org.acme.shipping.perishable.Shipper#shipper@email.com'
composer card import --file shipper1.card

composer identity issue --card admin@iot-perishable-network-advanced --file importer1.card --newUserId importer1 --participantId 'resource:org.acme.shipping.perishable.Importer#supermarket@email.com'
composer card import --file importer1.card

composer identity issue --card admin@iot-perishable-network-advanced --file sensor_temp1.card --newUserId sensor_temp1 --participantId 'resource:org.acme.shipping.perishable.TemperatureSensor#SENSOR_TEMP001'
composer card import --file sensor_temp1.card

composer identity issue --card admin@iot-perishable-network-advanced --file sensor_gps1.card --newUserId sensor_gps1 --participantId 'resource:org.acme.shipping.perishable.GpsSensor#SENSOR_GPS001'
composer card import --file sensor_gps1.card

```

#### submit transactions
```
composer transaction submit --card grower1@iot-perishable-network-advanced -d '{"$class": "org.acme.shipping.perishable.ShipmentPacked", "shipment": "resource:org.acme.shipping.perishable.Shipment#SHIP_001"}'

composer transaction submit --card shipper1@iot-perishable-network-advanced -d '{"$class": "org.acme.shipping.perishable.ShipmentPacked", "shipment": "resource:org.acme.shipping.perishable.Shipment#SHIP_001"}'


composer transaction submit --card shipper1@iot-perishable-network-advanced -d '{"$class": "org.acme.shipping.perishable.ShipmentPickup", "shipment": "resource:org.acme.shipping.perishable.Shipment#SHIP_001"}'

composer transaction submit --card shipper1@iot-perishable-network-advanced -d '{"$class": "org.acme.shipping.perishable.ShipmentLoaded", "shipment": "resource:org.acme.shipping.perishable.Shipment#SHIP_001"}'


composer transaction submit --card sensor_temp1@iot-perishable-network-advanced -d '{ "$class": "org.acme.shipping.perishable.TemperatureReading", "centigrade": 2, "shipment": "resource:org.acme.shipping.perishable.Shipment#SHIP_001"}'
 
composer transaction submit --card sensor_temp1@iot-perishable-network-advanced -d '{ "$class": "org.acme.shipping.perishable.TemperatureReading", "centigrade": 3, "shipment": "resource:org.acme.shipping.perishable.Shipment#SHIP_001"}'

composer transaction submit --card sensor_gps1@iot-perishable-network-advanced -d '{"$class": "org.acme.shipping.perishable.GpsReading", "readingTime": "2200", "readingDate": "20171118", "latitude": "40.6840", "latitudeDir": "N", "longitude": "74.0062", "longitudeDir": "W", "shipment": "resource:org.acme.shipping.perishable.Shipment#SHIP_001"}' 

composer transaction submit --card importer1@iot-perishable-network-advanced -d '{"$class": "org.acme.shipping.perishable.ShipmentReceived", "shipment": "resource:org.acme.shipping.perishable.Shipment#SHIP_001"}'

```
composer-rest-server

curl -X GET --header 'Accept: application/json' 'http://host1.scray.org:9000/api/org.acme.shipping.perishable.Shipment'


```
{
  "$class": "org.acme.shipping.perishable.TemperatureReading",
  "centigrade": 0,
  "shipment": "org.acme.shipping.perishable.Shipment#SHIP_001"
}
```

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{ \ 
   "$class": "org.acme.shipping.perishable.TemperatureReading", \ 
   "centigrade": 0, \ 
   "shipment": "org.acme.shipping.perishable.Shipment#SHIP_001" \ 
 }' "http://localhost:9999/api/org.acme.shipping.perishable.TemperatureReading"

curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{ "$class": "org.acme.shipping.perishable.TemperatureReading", "centigrade": 0,  "shipment": "org.acme.shipping.perishable.Shipment#SHIP_001" }' "http://localhost:9999/api/org.acme.shipping.perishable.TemperatureReading"
 
```

#### node red

https://medium.com/@CazChurchUk/integrate-your-blockchain-with-anything-using-hyperledger-composer-and-nodered-4226676f7e54

https://www.ibm.com/developerworks/cloud/library/cl-deploy-interact-extend-local-blockchain-network-with-hyperledger-composer/index.html?ca=drs-

https://nodered.org/docs/getting-started/installation


```
sudo apt-get install nodejs 
sudo apt-get install npm 
sudo npm install -g --unsafe-perm node-red
sudo rm -rf /usr/local/lib/node_modules/npm-check-updates/ 

/home/ubuntu/.nvm/versions/node/v8.9.2/bin/node-red
ssh -L 1880:localhost:1880 ubuntu@10.15.24.231
```

```

```


https://stackoverflow.com/questions/47668922/what-is-the-relation-between-fabric-composer-cello-and-other-hyperledger

Fabric provides a framework to set up a blockchain network. It is data/application agnostic.

Composer provides a set of tools to define a business network on top of Fabric. This provides a higher level of abstraction than Fabric where the data are essentially just bits. With Composer you can define assets, transactions, etc.

Cello helps with provisioning of the network.

Explorer simply provides a web based interface to explore what's on a blockchain.


* Deploying a Hyperledger Composer blockchain business network to Hyperledger Fabric for a single organization
https://hyperledger.github.io/composer/tutorials/deploy-to-fabric-single-org.html

curl -O https://hyperledger.github.io/composer/prereqs-ubuntu.sh
chmod u+x prereqs-ubuntu.sh


https://stackoverflow.com/questions/44779648/create-one-more-peer-node-using-hyperledger-composer
