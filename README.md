# Radicale Docker compose project 
Steps to use radicale docker compose file from official radicale docker, the offical website https://radicale.org/

## Step 1
create folder radicale
```bash
mkdir -p $HOME/radicale
cd $HOME/radicale
```
## Step 2
```bash
wget https://raw.githubusercontent.com/Kozea/Radicale/master/Dockerfile
```
## Step 3
```bash
docker build -t zxuradicale:3.1.6 .
```
## Step 4
Prepare config, user, rights
Access the htpasswd online tool https://tool.oschina.net/htpasswd to generate MD5 password. The format username:password will be used.
```bash
mkdir -p $HOME/radicale/{etc,collections}

echo 'username:$apr1$OcPL.lFw$KBVFjrNTksW0exp5TeKjj.' > $HOME/radicale/etc/users

cat << EOF > $HOME/radicale/etc/config
[server]
hosts = 0.0.0.0:5232

[encoding]
request = utf-8
stock = utf-8

[auth]
type = htpasswd
htpasswd_filename = /etc/radicale/users
htpasswd_encryption = md5

[rights]
type = from_file
file = /etc/radicale/rights

[storage]
filesystem_folder = /var/lib/radicale/collections
EOF

cat << EOF > $HOME/radicale/etc/rights
[root]
user: .+
collection:
permissions: R

[principal]
user: .+
collection: {user}
permissions: RW

[calendars]
user: .+
collection: {user}/[^/]+
permissions: rw
EOF
```
## Step 5
```bash
wget https://raw.githubusercontent.com/ZachXu/zxuradicale/main/docker-compose.yml
```
## Step 6
```bash
docker-compose up -d
```
