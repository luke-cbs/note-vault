## Disk Usage

Shows metrics for our usage metrics

```sh
df -h
```

## Restart a service and check status of a service

```sh
systemctl restart {service}
systemctl status {service}
```

## Install Ping

```sh
apt-get update && apt-get install iputils-ping
```

### Update Snap store

```shell
killall snap-store
sudo snap refresh snap-store
```