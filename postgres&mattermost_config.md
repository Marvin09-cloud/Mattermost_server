## POSTGRESQL CONFIGURATION AND MATTERMOST INSTALLATION

**STEP 1 :** 
First of all, you'll need to create and add the mattermost server PPA repository paquages. Refer to the official mattermost server to do it :

<https://docs.mattermost.com/install/install-ubuntu.html>

**STEP 2 :**
Then, before installing mattermost, you must config postgres.
If you haven't already done so, install postgres first :
```
sudo apt install postgresql
sudo apt install postgresql-client-common
sudp apt install postgresql-client-16
sudo apt-get update
```
Checking if postgresql client is well set up 
```
 psql --version
```

Checking if postgresql service is well set up :
```
sudo systemctl status postgresql
```

**STEP 3 :**  Postgres configuration
After installing postgresql and his client, you'll must create a postgres user using the following command :
```
sudo useradd -m -s /bin/bash postgres
```

And type his new password :
```
sudo passwd postgres
```
Accessing postgres :
```
sudo su - postgres
```
Configure your mattermost database :
```
psql
```
Then put this :
```**STEP 3 :** 
CREATE ROLE root WITH SUPERUSER LOGIN;
CREATE ROLE mmuser WITH SUPERUSER;
ALTER ROLE mmuser WITH PASSWORD 'x';
CREATE DATABASE mattermost;
```
**STEP 4 :** Installing mattermost
```
sudo apt update
sudo apt install mattermost -y
```

## Setup
```
sudo install -C -m 600 -o mattermost -g mattermost /opt/mattermost/config/config.defaults.json /opt/mattermost/config/config.json
```
After all of this, you will just have to be careful that your config.json file(`/opt/mattermost/config/config.json`) be conform to your postgresql table created early :

\-Set *DataSource* to "postgres://mmuser:"mmuser-password"@"host-name-or-IP":5432/mattermost?sslmode=disable&connect_timeout=10" replacing mmuser, "mmuser-password", "host-name-or-IP" and mattermost with your database name

\-Set *SiteURL*: The domain name for the Mattermost application

After modifying the `config.json` configuration file, you can now start the Mattermost Server:
```
sudo systemctl start mattermost
```
The final step, depending on your requirements, is to run `sudo systemctl enable mattermost.service` so that Mattermost will start on system boot.

## Updates
When a new Mattermost version is released, run: `sudo apt update && sudo apt upgrade` to download and update your Mattermost instance.

## Remove Mattermost
If you wish to remove the Mattermost Server for any reason, you can run this command:
```
sudo apt remove --purge mattermost
```
