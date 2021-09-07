# Marie Kondo a Machine

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



