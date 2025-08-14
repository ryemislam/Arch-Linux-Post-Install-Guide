#### Create folders for the drives
```
sudo mkdir /mnt/data-linux
sudo mkdir /mnt/games-linux
sudo mkdir /mnt/backup-linux
```

#### Find the UUID of the drives
```
sudo blkid
```

#### Edit fstab
```
sudo micro /etc/fstab
```

#### Add the drives to fstab
```
#data-linux
UUID=6684f4db-edfd-4d4a-8792-87d803c50b51   /mnt/data-linux    xfs    defaults    0    0
#games-linux
UUID=1035265b-29f4-420f-a6d3-18a69c107ec5   /mnt/games-linux    xfs    defaults    0    0
#backup-linux
UUID=d9a34659-f05e-4a90-b90b-f3eecf3854f2   /mnt/backup-linux    xfs    defaults    0    0
```

#### Mount the drives
```
sudo mount -a
systemctl daemon-reload
```
