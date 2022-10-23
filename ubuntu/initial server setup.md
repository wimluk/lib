# Ubuntu initial server setup

create the server

log into your server with ssh

```
ssh root@yourip
```
update your server
```
apt-get update
apt-get upgrade
```
create a user with sudo permission
```
adduser admin
usermod -aG sudo admin
```
reboot the server and log in with your user
```
reboot
```
```
ssh admin@yourip
```
initial server setup is finished