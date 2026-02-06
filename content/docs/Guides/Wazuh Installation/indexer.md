---
weight: 203
title: "Indexer Installation"
description: ""
icon: "article"
date: "2026-02-04T09:38:26+03:00"
lastmod: "2026-02-04T09:38:26+03:00"
draft: false
toc: true
---


### Certificate creation

Wazuh uses certificates to establish confidentiality and encrypt communications between its central components.

```bash

curl -sO https://packages.wazuh.com/4.14/wazuh-certs-tool.sh
curl -sO https://packages.wazuh.com/4.14/config.yml
```

Edit `./config.yml` and replace the node names and IP values with the corresponding names and IP addresses.

Be keen on spaces while editing the  ./config.yml
indexer IP: 192.168.104.200

dashboard and manager IP: 192.168.104.199

Run `./wazuh-certs-tool.sh` to create the certificates.

```bash
 ./wazuh-certs-tool.sh -A
```

Compress all the necessary files.

```bash
tar -cvf ./wazuh-certificates.tar -C ./wazuh-certificates/ .
rm -rf ./wazuh-certificates
```

Copy the wazuh-certificates.tar file to all the nodes, including the Wazuh indexer, Wazuh server, and Wazuh dashboard nodes. This can be done by using the scp utility.

```bash
 scp wazuh-certificates.tar wazuh@192.168.104.199:/home/wazuh
 ```

Run the following command to install the following packages if missing:

```bash
yum install coreutils
```

### Adding the wazuh repository

Import the GPG key

```bash
rpm --import https://packages.wazuh.com/key/GPG-KEY-WAZUH
```
Add the repository

```bash
echo -e '[wazuh]\ngpgcheck=1\ngpgkey=https://packages.wazuh.com/key/GPG-KEY-WAZUH\nenabled=1\nname=EL-$releasever - Wazuh\nbaseurl=https://packages.wazuh.com/4.x/yum/\npriority=1' | tee /etc/yum.repos.d/wazuh.repo
```

Install wazuh indexer

```bash
dnf -y install wazuh-indexer
```

You can edit the /etc/wazuh-indexer/opensearch.yml to setup multiple indexer nodes in a Prod, DR setup

Run this command replacing <INDEXER_NODE_NAME> with the name of the Wazuh indexer node you are configuring as defined in config.yml 

```bash
NODE_NAME=node-1
echo $NODE_NAME

mkdir /etc/wazuh-indexer/certs
tar -xf ./wazuh-certificates.tar -C /etc/wazuh-indexer/certs/ ./$NODE_NAME.pem ./$NODE_NAME-key.pem ./admin.pem ./admin-key.pem ./root-ca.pem
mv -n /etc/wazuh-indexer/certs/$NODE_NAME.pem /etc/wazuh-indexer/certs/indexer.pem
mv -n /etc/wazuh-indexer/certs/$NODE_NAME-key.pem /etc/wazuh-indexer/certs/indexer-key.pem
chmod 500 /etc/wazuh-indexer/certs
chmod 400 /etc/wazuh-indexer/certs/*
chown -R wazuh-indexer:wazuh-indexer /etc/wazuh-indexer/certs
```

```bash
systemctl daemon-reload
systemctl enable wazuh-indexer
systemctl start wazuh-indexer
```

You can check if it’s up and running

```bash
systemctl status wazuh-indexer
```

Disable updates, recommended to prevent accidental upgrades

```bash
sed -i "s/^enabled=1/enabled=0/" /etc/yum.repos.d/wazuh.repo
```

Cluster Initialization - Run the Wazuh indexer indexer-security-init.sh script on any Wazuh indexer node to load the new certificates information and start the single-node or multi-node cluster.

```bash
/usr/share/wazuh-indexer/bin/indexer-security-init.sh
```

Test that the installation was successful

```bash
curl -k -u admin https://192.168.104.200:9200
```

Run the following command to check if the cluster is working correctly. Replace <WAZUH_INDEXER_IP_ADDRESS> with the IP address of the Wazuh indexer and enter admin as the password when prompted:

```bash
curl -k -u admin https://192.168.104.200:9200nodes?v
```

### Challenges

You cannot access the interface outside the indexer server, the ports are not enabled in the firewall

```bash
firewall-cmd --add-port=9200/tcp --permanent 
firewall-cmd --list-all
firewall-cmd --list-ports 
firewall-cmd --reload
```
Adds the 9200 port to the firewall permanently and reloads

Node now works correctly
