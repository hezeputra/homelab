1. edit the lxc container

```
nano /etc/pve/lxc/101.conf
```

2. Giving the LXC access to GPU

```
lxc.cgroup2.devices.allow: c 226:0 rwm
lxc.cgroup2.devices.allow: c 226:128 rwm
lxc.mount.entry: /dev/dri/renderD128 dev/dri/renderD128 none bind,optional,create=file
```

3. giving the priviledge to the GPU Driver

```
chmod 777 /dev/dri/renderD128
```

4. install curl

```
apt install curl -y
```

5. install jelly fin

```
curl https://repo.jellyfin.org/install-debuntu.sh | sudo bash
```
