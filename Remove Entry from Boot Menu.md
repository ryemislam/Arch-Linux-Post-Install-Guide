##### Enter this to the terminal to list all the entries in Boot Menu

```
sudo efibootmgr
```

Should show BootOrder like Boot0000, Boot0001, Boot0002
##### Enter this to delete the entry from Boot Menu. E.g. sudo efibootmgr -b 1 -B
```
sudo efibootmgr -b <boot number> -B
```
For example:
sudo efibootmgr -b <1> -B

-b: modify boot number
-B: delete boot number
