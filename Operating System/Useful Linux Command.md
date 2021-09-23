# Mount Disk

## Check disk

Check if the external disk is detected by the system.

``` shell
fdisk -l
```

If there is a device shown as something like `Disk /dev/sdb`, then the disk is detected.

## Create folder for mounting

Create folder under `/mnt` for the external disk to mount (need administrator permission).

``` shell
sudo mkdir /mnt/usb
```

## Mount disk

Mount the disk (using the exact name shown in `fdisk -l`) to the folder, and then the disk can be visited.

``` shell
sudo mount /dev/sdb /mnt/usb
```

# Unmount disk

``` shell
sudo umount /mnt/usb
```

# Copy file

If copy file, better use `-i` option for alerting overwritting target file.

``` shell
cp -i filename /originalPath /targetPath
# If want to rename file
cp -i /originalFile /targetFile
```

If copy folder, better use `-a` option for recursively copying all files under the folder.

``` shell
cp -a /originalFolder /targetFolder
```