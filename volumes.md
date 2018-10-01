---
title: Volumes
permalink: /volumes
icon: hdd
order: 05
---

Volumes are extra storage you can attach to your virtual machine.

Let's take a look at the current status of our virtual machine, by using the `lsblk` tool. This tool will show any block devices available.

```
lsblk
```

```console
NAME   MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
vda    252:0    0  10G  0 disk
└─vda1 252:1    0  10G  0 part /
```

From the output above, we can see that we have a single 10GB disk available called `vda`. This is the root disk that is provided by our Instance flavor we chose in an earlier step. This block device has one partition `vda1` which is mounted to `/`, which is our root filesystem.

Using the `df` tool, we can get a better look at the filesystem. We pass the `-h` argument to show us _human readable_ sizes and `/` as the last argument to only show us the root filesystem mounted at `/`.

```
df -h /
```

```console
Filesystem      Size  Used Avail Use% Mounted on
/dev/vda1       9.9G  1.1G  8.4G  12% /
```

So we currenly have `8.4G` of storage available on our new Instance. Depending on the type of workload that this instance might run, that might not be enough.

## Creating a new volume

To create a new volume, select [Project > Volumes -> Volumes](https://dashboard.rc.nectar.org.au/project/volumes/) from the left-hand navigation panel. Click the `Create Volume` button to get started.

From the `Create Volume` form, you should specify a `Name` and a `Size` for your new volume. It is also very important to select the `Availability Zone` that matches the Availability Zone that your Instance has been launched in. A Volume can only be attached to an Instance if their Availability Zones match.

## Attaching the volume

Once the Volume has been created, click the down arrow of `Actions` menu next to your new volume, and choose `Manage Attachments`. From the form, you can choose to attach the volume to your instance. Find your Instance from the list, and click the `Attach` button.

The status should change to indicate the Volume has now been attached to your new instance, and this should be reflected within our Instance.

```
lsblk
```

```console
NAME   MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
vda    252:0    0  10G  0 disk
└─vda1 252:1    0  10G  0 part /
vdb    252:16   0   1G  0 disk
```

As we can now see, there is a new 1GB disk attached called `vdb` with no partitions.

## Creating a filesystem

To make this storage usable, we need to create a filesystem on this device, if one doesn't already exist. In this case, we're certain that the device doesn't already contain a filesystem.

We're going to use the `lsblk` command again, but pass the `--fs` option to show us any filesystem information.

```
lsblk --fs
```

```console
NAME   FSTYPE LABEL UUID                                 MOUNTPOINT
vda
└─vda1 ext4         3bd4d5a4-a8ce-4584-acda-28d323c0ec99 /
vdb
```

This shows us that `vdb` does not have a filesystem on it yet, so we should go ahead and create one. We're going to use the standard `ext4` filesystem for our Volume. There are other choices we could make, depending on our requirements, but `ext4` is a good choice for general workloads. Note that we need to prefix this command with `sudo` as we'll require superuser privileges.

```
sudo mkfs -t ext4 /dev/vdb
```

```console
mke2fs 1.44.1 (24-Mar-2018)
Creating filesystem with 262144 4k blocks and 65536 inodes
Filesystem UUID: cd8d17cb-20f2-469b-9ff5-8ab2d1db2856
Superblock backups stored on blocks:
	32768, 98304, 163840, 229376

Allocating group tables: done
Writing inode tables: done
Creating journal (8192 blocks): done
Writing superblocks and filesystem accounting information: done
```

Running our `lsblk` command again shows that we've successfully created a filesystem on our device.

```
lsblk --fs
```

```console
NAME   FSTYPE LABEL UUID                                 MOUNTPOINT
vda
└─vda1 ext4         3bd4d5a4-a8ce-4584-acda-28d323c0ec99 /
vdb    ext4         cd8d17cb-20f2-469b-9ff5-8ab2d1db2856
```

Take note of that `UUID` value for our new filesystem on the `vdb` device.

## Mounting the volume

The first step is to prepare our mount point. This will be the location to where our new Volume's filesystem will be available. In this example, we're going to use the path `/mnt/data`.

We're going to create directory for this using the `mkdir` command. The `-p` argument creates any preceeding directories if they don't already exist, and the last argument is the path to create.

```
sudo mkdir -p /mnt/data
```

Let's now mount our filesystem to this new path, using the mount command. We need to pass it the device (which is `vdb`) and the path to where it should be mounted.

```
sudo mount /dev/vdb /mnt/data
```

Coming back to our `df` tool, we can now see the status of the filesystem we created on our Volume now mounted and available for use.

```
df -h /mnt/data
```

```console
Filesystem      Size  Used Avail Use% Mounted on
/dev/vdb        976M  2.6M  907M   1% /mnt/data
```

## Auto mounting our filesystem

To ensure the filesystem is mounted on boot, we need to add an entry for it to the `/etc/fstab` file.

Our entry is in the following format:

```console
device_name  mount_point  file_system_type  fs_mntops  fs_freq  fs_passno
```

So in our case, we'll add the following line to our `/etc/fstab` file.

```console
UUID=cd8d17cb-20f2-469b-9ff5-8ab2d1db2856   /mnt/data  ext4  defaults,nofail  0  2
```

The UUID is the value we discovered earlier corresponding to our new filesysten on `vdb`. Explaining all the values here are out of scope for this example, but if you're interested in what these values mean, you should consult the `fstab` manual page by typing `man fstab`.

Now, we'll edit the file using the `nano` editor with superuser priviliges.

```
sudo nano /etc/fstab
```

Add our new entry to the bottom of the file. Once you're happy, press `Ctrl+X` to exit, and type `Y` to save, and hit `enter` to accept the default file name.

To confirm the changes to the file are valid, we can run the `mount -a` command, again with superuser priviliges.

```
sudo mount -a
```

If everything is correct, you should see no output.
