# Scenario
You have installed linux in a single partition but you later discovered that it would be better to keep `/home` directory on seperate volume to avoid the situation where the system processes will run out of space because of users data. The same decision applies to `/var`, though the motive is different.

In this lab, you'll create two data volumes in a new disk and then use them to store data from `/home` and `/var` partitions.
 
# Requirements
- this is lab is to be carried on a clean ubuntu server installation.
- always take a snapshot before starting your virtual machine.


# Tasks

## Find information on your disk device
- power on your vm
- find the following informations using commands `lsblk -f`, `sudo fdisk -l`, `sudo parted -l`, `findmnt`:
  - the disk device connected to your system. (you'll typically have one disk device)
  - the partitions' names and the partitionning scheme.
  - the mounted partitions, for each one: the partition name, mount point and the filesystem type.

- find the remaining free space in each of the partitions `df -h`

## Add a disk to the virtual machine
- power off your vm
- add a hard disk to your vm from the vmware/virtualbox menu (look for vm settings). the virtual disk must have the following specifications:
  - storage format: VHD
  - size: 1GB, dynamically growing size (which means that the actual size of the file representing the disk will grow as needed up to 1GB)
  - disk interface: SATA
  - make the disk a single file (with larger disks, it may be useful to let vmware/virtualbox split the disk file into smaller slices, for instance, to make copying it to other locations easier using a flash drive)

## Prepare the disk for use
- power on your vm
- check that the new disk is detected by your kernel using `dmesg | grep sd.`
  - what does `dmesg` actually show?
- what is the name given to the new disk?
- find out which information can you get on the new disk device: names, size in blocks, the used block size, etc.

- make two partitions on the new disk using `cfdisk`, a disk partitionning tool with a screen-based interface (non command line one). follow [this video](https://youtu.be/jYbGyHvx7EQ?t=360) to learn how to use its menu. you'll have to go through the following steps:
  - select the dos partitionning scheme (aka disklabel)
  - create two non-bootable, primary partitions: the first is 700MB and the second uses the remaining space
  - save the changes (write them on disk) and quit.
  
  note: it would be possible to use `fdisk` or `parted` instead of `cfdisk`. this one was suggested because it looks easier for a beginner.

- now, find the names, labels, and uuid of the created partitions using `fdisk -l /dev/sdb` of `lsblk -f /dev/sdb` (assuming that `sdb` is the new disk name as found in `dmesg` output)

- the created partitions are not usable yet because no filesystems are created on them, so let's go on: (assuming that sdb is the new device name)
  - use `mkfs.ext4 -L home /dev/sdb1` to install an ext4 filesystem on the first partition. option `L` sets the label of the obtained volume.
  - use `mkfs.ntfs -L var /dev/sdb2` to install an ntfs filesystem on the second one.

  note: on windows environments, creating a filesystem of type ... on a partition translates into *formatting partitions as ...*)

- find what changed in the `sudo fdisk -l /dev/sdb` of `lsblk -f` output. you may also test `sudo parted -l /dev/sdb` 

- the last step to use the created ext4 and ntfs volumes is to mount them on the root filesystem, that is, assign them a path within the root filesystem. for this purpose, you have to:
  - create directories that will serve as mount points `/mnt/home` and `/mnt/var`. hard disk volumes and remote volumes are typically mounted in `/mnt`, while removable media (cd, usb, etc.) are mounted in `/media`. the directories should belong to root:root and have 755 permissions.
  - mount them using `mount -t ext4 /dev/sdb1 /mnt/home`. the same for `/mnt/var`

- find the free space in `/dev/sdb1` using `df -h`. Do we have the 700MB created during partitionning? Can you find out why?

- you may now start using them by creating some files in them.

## Migrating /home and /var to seperate volumes
- we are now ready to migrate the `/home` directory to `/mnt/home` using `sudo cp -arT /home /mnt/home`.
  - find what each options means.
  - does `cp` copy the metadata with fhe file data? does it keep the same inode number?

- now, unmont the volume using `umount /mnt/home`; the content is no longer accessible but it does exist.
- next, empty the `/home` directory using `rm -Rf /home/\*`. keep an empty `/home` directory.
- finally remount the volume labeled `home` under the mount point `/home`. you can identify the volume with its label instead of the device name as in `mount -t ext4 -L home /home`
- do the same thing with the other volume. Remount it under `/var`

## Setting automatic mount at system startup
- the `home` and `var` partitions were manually mounted, from the command line. they will remain accesible until the system reboots or you unmout them using `umount`. to keep the volumes mounted across reboots, you must set this up by adding the following two lines to the `/etc/fstab` file (file system table) 
>>    /dev/sdb1 /home ext4 defaults 0 2
>>    /dev/sdb2 /var ntfs defaults 0 2
this means that `/dev/sdb1` partition will be mounted at `/home`, it is of type `ext4`, and is mounted with the default options (user, rw, etc.) the last field is the priority with wich the volume is handled by `fsck` command.
- test your configuration using `mount /dev/sdb1` or `mount /home`. then test `mount -a` (meaning, mount all volumes defined in `/etc/fstab`)
- if every thing goes ok, reboot the system and check the mounts