# motd-dynamicbanner
Message of the Day dynamic banner. 

Most of this comes form:
- https://chrisrmiller.com/ubuntu-dynamic-motd-on-debian-9/
- https://nickcharlton.net/posts/debian-ubuntu-dynamic-motd.html

With small bits of changes to get it to work on Debian 11 (Buster), and with python3.

#### 0. make Debian check for updates (automatically)
https://wiki.debian.org/UnattendedUpgrades
```
sudo apt install unattended-upgrades apt-listchanges
sudo editor /etc/apt/apt.conf.d/50unattended-upgrades
```
enable (remove //):
```
      "origin=Debian,codename=${distro_codename}-updates";
      "origin=Debian,codename=${distro_codename}-proposed-updates";
```
Save file.

Make sure that we only check for updates, do not install them.
```
cat <<EOF | sudo tee /etc/apt/apt.conf.d/20auto-upgrades > /dev/null
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Unattended-Upgrade "0";
EOF
```
Enable the service.
```
sudo systemctl enable unattended-upgrades.service
```
Test it ...
```
sudo unattended-upgrade -d
```


#### 1. install a few packages

```
sudo apt install lsb-release figlet
```
'figlet' is the neat thing which draws text graphics.

```
# figlet Hello World
 _   _      _ _        __        __         _     _
| | | | ___| | | ___   \ \      / /__  _ __| | __| |
| |_| |/ _ \ | |/ _ \   \ \ /\ / / _ \| '__| |/ _` |
|  _  |  __/ | | (_) |   \ V  V / (_) | |  | | (_| |
|_| |_|\___|_|_|\___/     \_/\_/ \___/|_|  |_|\__,_|
```

#### 2. create the directory
```
sudo mkdir /etc/update-motd.d
cd /etc/update-motd.d
sudo touch 00-header && sudo touch 10-sysinfo && sudo touch 90-footer
```

#### 3. do a few things to make motd dynamic
sudo 


Working in progress.