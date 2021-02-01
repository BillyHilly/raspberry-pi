If you are connected to the Linux machine, in this case a Raspberry Pi running Raspbian (debian linux) with a terminal window or via ssh use `fdisk` to view and recreate the external hard drive partitions. 

##make sure you are `root` ( `su - root` or `sudo -s` gets the job done)

`sudo -s fdisk /dev/sda`
while in fdisk you can verify the partitions with the the `p` command
```
Command (m for help): p
Disk /dev/sda: xxx GB, xxx bytes
```
1. Use the `d` command to delete existing partition, then `p` again to verify it is gone
2. Use the `n` command creates a new partition,
3. Use `p` for primary partition, `Enter` to default to partition 1, 
4. `Enter` to select first sector 
5. and `Enter` again to select last sector. This gives the whole disk to your new partition.
6. Now use the `p` command again to see your new partition
    ```
     Command (m for help): p
     Device Boot      Start         End      Blocks   Id  System
     /dev/sda1         2048   234441647   117219800   83  Linux
    ```
7. The changes need to be written to the partition table, so use the `w` command to commit.
8. Run the `sudo -s fdisk -l` command to see your disk which will now include /dev/sda1
9. Now you can make your file system. Use the `mkfs` command.
    ```
     mkfs /dev/sda1
    ```
 10. After the superblocks are created — may take a hot minute — you get a prompt you are ready to mount your disk create a mount point.
   Say you want it to be `mydisk`
    ```
     mkdir /mydisk
    ```
11. Now mount it
    ```
     mount /dev/sda1 /mydisk
    ```
12. use `df` to verify disk is mounted. 
    If you reboot you will need to remount it (you might want to add it to `/etc/fstab`)
13. Try writing a file to the disk
    ```
     touch /diskname/test
    ```
    A file should be created. If you get an error, follow it's lead or simply start over.
    

## Mounting
`sudo mount /dev/sda1 /mnt`
I got it to mount and allow access with this. Set up the directory to mount it to:
```
sudo mkdir /mnt/your-folder-name
sudo chown -R pi:pi /mnt/your-folder-name
sudo chmod -R 775 /mnt/your-folder-name
sudo setfacl -Rdm g:pi:rwx /mnt/your-folder-name
sudo setfacl -Rm g:pi:rwx /mnt/your-folder-name
```
Them mount it.
```
sudo mount -o uid=pi,gid=pi /dev/sda1 /mnt/your-folder-name
```
