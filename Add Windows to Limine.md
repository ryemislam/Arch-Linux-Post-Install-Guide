1. Install  the following packages:

```
paru -S limine-mkinitcpio-hook
```

2. Create or edit `/etc/default/limine` (You can optionally copy default settings from `/etc/limine-entry-tool.conf`):

```
sudo cp /etc/limine-entry-tool.conf /etc/default/limine
```

3. Check if your ESP is detected by:
```
bootctl --print-esp-path
```

- If detected, no further configuration is required. (It will show /boot)
- If not detected, set `ESP_PATH=` to point to any FAT32 boot partition of your choice, then run `limine-install` to verify.

4. Run this command to regenerate initramfs or UKIs and add (update) kernel boot entries to Limine:
```
sudo limine-update
```

5. Run this command to detect existing UEFI boot entries (dual-boot) and add them to Limine:
```
sudo limine-scan
```
