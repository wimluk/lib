# How to use letsencrypt on your ubuntu server

sudo apt install snapd
sudo snap install core; sudo snap refresh core
sudo snap install --classic certbot

sudo ln -s /snap/bin/certbot /usr/bin/certbot

sudo certbot certonly --standalone -d kafka-broker-3.nmon.at

sudo certbot certificates

here you can edit the auto renew:

sudo nano /etc/systemd/system/snap.certbot.renew.timer
sudo nano /etc/systemd/system/snap.certbot.renew.service

Certbot's certificates are only accessible by root. To allow kafka to use the certificate, it must create a copy with a Certbot renewal hook.

If we also need to convert the privkey to be password protected we can add it to our renewal hook. Before we create the hook file. We create a password and store it as environment variable.

You can use the following command to generate a random password:

```
openssl rand -base64 32
```

Now we store it:

```
export PRIVKEYPASSWORD="MGU1NGM4ZmQ1M2VkOTMwYjU2MDY3MzUw"
```

Create the renewal hook file.

```
sudo nano /etc/letsencrypt/renewal-hooks/deploy/<some name>.deploy
```

Configure the file:

```
#!/bin/bash

umask 0177

DOMAIN=<some domain>

DATA_DIR=<some path>

PRIVKEYPASSWORD=${PRIVKEYPASSWORD}

cp /etc/letsencrypt/live/$DOMAIN/fullchain.pem $DATA_DIR/fullchain.pem

cp /etc/letsencrypt/live/$DOMAIN/privkey.pem $DATA_DIR/privkey.pem

chown admin:admin $DATA_DIR/fullchain.pem $DATA_DIR/privkey.pem

openssl rsa -aes192 -in $DATA_DIR/privkey.pem -out $DATA_DIR/privkey.pem -passout env:PRIVKEYPASSWORD

```

Give the file executable permission.

```
sudo chmod +x /etc/letsencrypt/renewal-hooks/deploy/<some name>.deploy
```

Force a certbot renewal.

```
sudo certbot renew --force-renewal
```

If you can find fullchain.pem and privkey.pem at <some path> it worked.