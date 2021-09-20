# Use Ubuntu

## Hardware Management

### View all USB devices

```bash
sudo lsusb
```

### Unmount USB drive

```bash
# check available disks
sudo fdisk -l

# check mount points and find mount point of disk you want to unmount
sudo lsblk

# do the unmount (/path/to/mount is the path of the mount)
sudo umount /path/to/mount
# or using the path to the device instead, do
sudo udisksctl unmount -b /dev/sdx

# safely eject the device (/dev/sdx is the path to the device)
sudo udisksctl power-off -b /dev/
```





## Upgrading Ubuntu major versions

Run the following to trigger the upgrade via a GUI \(remove `-f` if you wanna be hardcore\)

```bash
sudo do-release-upgrade --allow-third-party -f DistUpgradeViewGtk3
```

## From docker

Run the following to clear space used by the Docker daemon:

```bash
# remove dangling images
docker prune images

# remove unused volumes
docker prune volumes
```

## From `journalctl`

Run the following to clear space from `/var/log/journal`:

```bash
sudo journalctl --rotate
sudo journalctl --vacuum-time=1d
```

## From `snapd`

Run the following to clear space from `/var/lib/snapd/cache/*`:

```bash
sudo rm /var/lib/snapd/cache/*
```



