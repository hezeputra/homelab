## Expand "local" pool by removing "local-lvm" pool

> [!IMPORTANT]
> Logical Volume Manager (LVM) is a technology that provides a flexible and dynamic way to manage storage space compared to traditional partitioning. It allows you to treat multiple physical storage devices as a single, larger virtual storage pool, offering advantages like resizing volumes without disrupting system operations.

## Remove "local-lvm" pool

1. Go to `datacenter`
2. Go to `storage`
3. remove `local-lvm`

## Assign removed volume to existing "local" pool

1. remove the `local-lvm` logical volume partition

```
lvremove /dev/pve/data
```

2. resize the `local` logical volume partition in `/dev/pve/root` that is obtained by remove `local-lvm` logical volume partition in `/dev/pve/data`

```
lvresize -l +100%FREE /dev/pve/root
```

3. Resize the EXT4 filesystem to make the filesystem use the new space that is allocated by logical volume from previous `lvresize` command

```
resize2fs /dev/mapper/pve-root
```

> [!IMPORTANT]
> Please replace `resize2fs` with `xfs_growfs` if you use XFS filesystem
