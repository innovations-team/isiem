---
weight: 203
title: "Manager Installation"
description: ""
icon: "article"
date: "2026-02-04T09:38:26+03:00"
lastmod: "2026-02-04T09:38:26+03:00"
draft: false
toc: true
---

We will be deploying the Wazuh server/manager and dashboard on the same server


Import GPG key and Add Wazuh repository

```bash
rpm --import https://packages.wazuh.com/key/GPG-KEY-WAZUH

echo -e '[wazuh]\ngpgcheck=1\ngpgkey=https://packages.wazuh.com/key/GPG-KEY-WAZUH\nenabled=1\nname=EL-$releasever - Wazuh\nbaseurl=https://packages.wazuh.com/4.x/yum/\npriority=1' | tee /etc/yum.repos.d/wazuh.repo
```

Install Wazuh manager package

```bash
dnf -y install wazuh-manager
```

Install filebeat 
```bash
dnf -y install filebeat
```

Download the preconfigured Filebeat configuration file and edit the /etc/filebeat/filebeat.yml , replace:

```bash
curl -so /etc/filebeat/filebeat.yml https://packages.wazuh.com/4.14/tpl/wazuh/filebeat/filebeat.yml
```
hosts - replace the IP address with you indexer IP 

Create filebeat keystore to securely store authentication credentials. 

Add the default username and password admin:admin to the secrets store

```bash
filebeat keystore create
echo admin | filebeat keystore add username --stdin --force
echo admin | filebeat keystore add password --stdin --force
```

Download the alerts template for the Wazuh indexer.

```bash
curl -so /etc/filebeat/wazuh-template.json https://raw.githubusercontent.com/wazuh/wazuh/v4.14.2/extensions/elasticsearch/7.x/wazuh-template.json
chmod go+r /etc/filebeat/wazuh-template.json
```

Install Wazuh module for Filebeat

```bash
curl -s https://packages.wazuh.com/4.x/filebeat/wazuh-filebeat-0.5.tar.gz | tar -xvz -C /usr/share/filebeat/module
```

### Deploying certificates
Ensure you have a copy of  `wazuh-certificates.tar` 

Replace  `<SERVER_NODE_NAME>` with your Wazuh server node certificate name, the same one used in `config.yml` when creating the certificates.

Move certificates to corresponding location

```bash
NODE_NAME=wazuh-1
echo $NODE_NAME

mkdir /etc/filebeat/certs
tar -xf ./wazuh-certificates.tar -C /etc/filebeat/certs/ ./$NODE_NAME.pem ./$NODE_NAME-key.pem ./root-ca.pem
mv -n /etc/filebeat/certs/$NODE_NAME.pem /etc/filebeat/certs/filebeat.pem
mv -n /etc/filebeat/certs/$NODE_NAME-key.pem /etc/filebeat/certs/filebeat-key.pem
chmod 500 /etc/filebeat/certs
chmod 400 /etc/filebeat/certs/*
chown -R root:root /etc/filebeat/certs
```

### Configuring Wazuh Indexer Connection

Save the Wazuh indexer username and password into the Wazuh manager keystore using the wazuh-keystore tool.

Replace `<WAZUH_INDEXER_USERNAME>` and `<WAZUH_INDEXER_PASSWORD>` with the Wazuh indexer username and password:

```bash
echo 'admin' | /var/ossec/bin/wazuh-keystore -f indexer -k username
echo 'admin' | /var/ossec/bin/wazuh-keystore -f indexer -k password
```

Edit `/var/ossec/etc/ossec.conf` to configure the indexer connection. 

Leave the IP as 0.0.0.0 to allow web interface access

Ensure the Filebeat certificate and key name match the certificate files in `/etc/filebeat/certs`.

Starting Wazuh Manager
```bash
systemctl daemon-reload
systemctl enable wazuh-manager
systemctl start wazuh-manager
systemctl status wazuh-manager
```

Enable and start the Filebeat service.
```bash
systemctl daemon-reload
systemctl enable filebeat
systemctl start filebeat
```

Verify Filebeat is successfully installed
```bash
filebeat test output
```

Disable updates
```bash
sed -i "s/^enabled=1/enabled=0/" /etc/yum.repos.d/wazuh.repo
```