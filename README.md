# mailserver-and-roundcube-with-docker-ubuntu
commands

```
sudo systemctl status docker
```

```
mkdir -p ~/mailserver/maildata
mkdir -p ~/mailserver/mailstate
mkdir -p ~/mailserver/config
cd ~/mailserver
```

```
docker run -d \
  --name mailserver \
  --hostname {hostname} \
  --domainname {domain name} \
  -p 25:25 \
  -p 587:587 \
  -p 993:993 \
  -v ~/mailserver/maildata:/var/mail \
  -v ~/mailserver/mailstate:/var/mail-state \
  -v ~/mailserver/config:/tmp/docker-mailserver \
  -e ENABLE_FAIL2BAN=1 \
  -e ENABLE_SPAMASSASSIN=1 \
  --restart always \
  mailserver/docker-mailserver:latest
```

```
cd ~/mailserver
wget https://raw.githubusercontent.com/docker-mailserver/docker-mailserver/master/setup.sh
chmod +x setup.sh
```

```
./setup.sh email add {name}@{your domain} {your password}
```

```
./setup.sh email list
```

```
docker run -d \
  --name roundcube \
  -p 8080:80 \
  --link mailserver \
  -e ROUNDCUBEMAIL_DEFAULT_HOST=mailserver \
  -e ROUNDCUBEMAIL_SMTP_SERVER=mailserver \
  --restart always \
  roundcube/roundcubemail
```

```
http://{your ip}:8080
```

