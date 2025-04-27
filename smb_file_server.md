1. Create new user

```
adduser admin
```

2. Add user into sudo group

```
usermod -aG sudo username
```

3. Modify the mount point ownership

```
chmod -R 0774 /media
chown -R admin:admin /media
```

4. Install samba

```
apt install samba -y
```

5. Create samba backup default configuration

```
sudo mv /etc/samba/smb.conf /etc/samba/smb.conf.backup
```

6. Createe new configuration

```
nano /etc/samba/smb.conf
```

```
[global]
   server string = media
   workgroup = WORKGROUP
   security = user
   map to guest = Bad User
   name resolve order = bcast host
   hosts allow = 192.168.1.0/22
   hosts deny = 0.0.0.0/0
[data]
   path = /media
   force user = admin
   force group = admin
   create mask = 0774
   force create mode = 0774
   directory mask = 0775
   force directory mode = 0775
   browseable = yes
   writable = yes
   read only = no
   guest ok = no
```

7. Add samba user

```
sudo smbpasswd -a admin
```

8. asdas

```
sudo systemctl enable smbd
```

```
sudo systemctl enable nmbd
```

```
sudo systemctl restart smbd
```

```
sudo systemctl restart nmbd
```

9. Windows auto discovery

```
apt install wsdd
```
