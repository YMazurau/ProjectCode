# PostgreSQL xxxx.20

* Install

```bash
apt update
apt -y install postgresql
```

* Create DB:

Login on user postgres
```
psql -U postgres -h localhost
```
Check DB list
```
\list
```
Create DB name "projectdb"
```
CREATE DATABASE projectdb;
```
Create username "projectdb" pass "xxx"
```
CREATE USER projectdb WITH PASSWORD 'xxx';
```
Assign a user permission to the DB using the GRANT command
```
GRANT ALL PRIVILEGES ON DATABASE projectdb TO projectdb;
```
Exit
```
\q
```


* Setting up a remote connection PostgreSQL

Check path of the configuration file postgresql.conf:

```
#user root
su - postgres -c "psql -c 'SHOW config_file;'"
```
```
NOTE:
 /etc/postgresql/12/main/postgresql.conf

```
Allowed listening to requests on all IP addresses and xxx.8 ( k3s)
```
nano /etc/postgresql/12/main/postgresql.conf
#listen_addresses = '*'
```
We allow remote connections to the server
```
nano /etc/postgresql/12/main/pg_hba.conf
```

Edit
```
# "local" is for Unix domain socket connections only
local   all             all                                     peer
# IPv4 local connections:
host    all             all             127.0.0.1/32            trust
# IPv6 local connections:
host    all             all             ::1/128                 trust
# Allow replication connections from localhost, by a user with the
# replication privilege.
local   replication     all                                     peer
host    replication     all             127.0.0.1/32            md5
host    replication     all             ::1/128                 md5
host    all             all             xxx.xxx.xxx.8/32        md5
host    all             all             0.0.0.0/0               md5
host    all             all             ::/0                    md5
```

Restart
```
systemctl restart postgresql-9.5
```

Check
```bash
psql -h 192.168.202.20 -U postgres
```

# Install Node.js

```bash
wget https://nodejs.org/dist/v20.10.0/node-v20.10.0-linux-x64.tar.xz
tar -xf node-v20.10.0-linux-x64.tar.xz
cd node-v20.10.0-linux-x64
sudo cp -R * /usr/local
node -v
sudo apt-get install npm 
#[https://typicode.github.io/husky/getting-started.html]
npx husky-init && npm install
```

# Install Husky

After each git commit, the version in the "package.json". The Git action assigns this version of the docker image.

```
WARNING! If you clone git repositories, delete the folder .husky/ and only then install
```
[https://typicode.github.io/husky/getting-started.html]


# Slack nitification  [https://github.com/rtCamp/action-slack-notify]

Create SLACK_WEBHOOK secret using GitHub Action's Secret. You can generate a Slack incoming webhook token from here.
[https://brosersstorage.slack.com/apps/A0F7XDUAZ-incoming-webhooks?tab=more_info]

```
! WARNING ! In the upper corner, be sure to check which chat the alert is configured in.
```

# Helm lint

Checks for errors with the notification in slack.
