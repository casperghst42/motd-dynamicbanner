# motd-dynamicbanner
Message of the Day dynamic banner. 
Feel free to clone and improve. Especially the 10-sysinfo could use some love.

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

#### 2. do the directory and files
Create the directory.
```
sudo mkdir /etc/update-motd.d
```
Get the files:
``` 
cd <>
git clone https://github.com/casperghst42/motd-dynamicbanner.git
cd motd-dynamicbanner/files
sudo cp * /etc/update-motd.d
sudo chmod +x /etc/update-motd.d/*
```
##### 2.1 two different sysinfo's
There are two different sysinfo's:
10-sysinfo
10-sysinfo-debian

Switch between thenm by adding or removing the executable flag.


#### 3. make sure that the orignal motd is not shown
```
sudo mv /etc/motd /etc/motd.orig
```


#### 4. maks sure that ssh does not show motd
```
sudo editor /etc/ssh/sshd_config
```
Change the following
```
PrintMotd no
PrintLastLog yes

#Banner /etc/issue
```

Restart sshd
```
sudo systemctl restart sshd
```

#### 5. test....
Login to your server using ssh:
```
 _ __   __ _ ___
| '_ \ / _` / __|
| | | | (_| \__ \
|_| |_|\__,_|___/


Welcome to Debian GNU/Linux 11 (bullseye) (5.10.0-19-amd64)

System information as of: Sun Nov 13 15:51:37 CET 2022

System load:	0.47		IP Address:	10.10.42.10
Memory usage:	26.7		System uptime:	5:14 hours
Usage on /:	19%		Swap usage:	0.0%
Local Users:	1		Processes:	333

Linux nasnode 5.10.0-19-amd64 #1 SMP Debian 5.10.149-2 (2022-10-21) x86_64
0 updates to install.
0 are security updates.

Last login: Sun Nov 13 14:46:56 2022 from ff80::1
```
It should produce something like this, but if it does not then figure out why ... 