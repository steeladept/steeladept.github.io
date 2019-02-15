---
title: Extending LVM Partitions
keywords: linux
last_updated: Feb 14, 2019
tags: [linux]
summary: "Linux file system provides many different ways to partition your disks. One of the newer, more flexible ways is using LVM. This article describes how to take advantage of that flexibility to extend your LVM partitions. This is taken step by step from [https://www.rootusers.com](https://www.rootusers.com/how-to-increase-the-size-of-a-linux-lvm-by-expanding-the-virtual-machine-disk/) and is reproduced here (using my examples) to ensure availability. Many more details, plus a video and other alternatives can be found at the site it was pulled from."
sidebar: linux_sidebar
permalink: lvmpartition.html
folder: linux
---

The examples here are done on CENTOS 7 using LVM.

## Identify Partition Type ##

First, we need to be sure we are dealing with LVM and not another partition system, otherwise we could screw things up and cause massive data loss!  

>You have been warned!

We determine the partition type by issuing the `fdisk -l` command.

![alt text:   fdisk -l example][fdisk]

Note the first line shows the disk name and size (in this case, /dev/sda size of 21.5GB). Also note, the last column in the table shows the partition type. Id 8e means LVM as does the System "Linux LVM". So in this machine, we can see that /dev/sda5 is an LVM partition.

## Add disk space ##

Once we know that, we need to actually increase the hard disk space - physically. Using VMware (or whatever virtualization software you are running on), make the logical size of the hard drive larger. If you are doing this using physical drives, it can be done, but it requires modification to this document so I am leaving that as outside the scope of this document.

While you can run a command to rescan and recognize the new drive space, I was unable to get this to work reliably.  Might be I needed a different path, different host or who knows; but one thing that ALWAYS works (and what I did) is to reboot the machine at this point.

## Partition new disk space ##

Once the new disk is recognized (test by running `fdisk -l` once again and verify the size shows in the top line for the drive in question), it is a simple process of creating the partition, adding it to the volume group, then extending the logical volume to include the new space. Note, this looks like it is creating another partition at first, because it is, but we are going to be okay and get to where we want to at the end of the document, just be patient and follow the steps....

The first step, creating the partition, is done by issuing `fdisk <device name>` where the new device name is something like /dev/sda. This drops you into the fdisk subsystem

```
fdisk /dev/sda
```

Once in the subsystem, you can type m for help and to see all that fdisk allows you to do.  In our case, we are starting with the `n` command for a new partition.

```
WARNING: DOS-compatible mode is deprecated. It's strongly recommended to                 switch off the mode (command 'c') and change display units to 
         sectors (command 'u').

Command (m for help): n
```

The next option is `l` for a logical partition or `p` for a primary partition. We are going to choose `p`. You only get 4 primary partitions, so if this is going to be your 5th, look elsewhere before you continue. You may be able to choose `l` instead, but I have never needed to and can not verify it will work. Proceed at your own risk if you do.

```
Command action
   l   logical (5 or over)
   p   primary partition (1-4)
p
```

Next, you will choose the partition number. Just keeping it logical, choose the next number in line - in my case it is 3.

```
Partition number (1-4): 3
```

Now there are a few questions about cylinders that I don't know anything about. All I know is I want to use all the new space allotted and that is what the default settings provide, so just hit `enter` for those questions.

```
First cylinder (2611-3916, default 2611): <enter>
Using default value 2611
Last cylinder, +cylinders or +size{K,M,G} (2611-3916, default 3916): <enter>
Using default value 3916
```

Finally, we need to ensure the new partition is LVM compatible, so press `t` at the command line and then choose the new partition number you just created.

```
Command (m for help): t
Partition number (1-5): 3
```

The Hex code it is asking for now is the one we spoke of at the beginning (the Id number 8e). You can see the full list by pressing `L`, or just take my word for it and type `8e`

```
Hex code (type L to list codes): 8e
Changed system type of partition 3 to 8e (Linux LVM)
```

So far, no matter how badly you screwed things up, we haven't made it permanent. Fdisk is a very dangerous tool, so they do leave escapes in case you FUBAR something. In this case, the final step is to write the new partition information into the file system. This is done with a simple `w` at the Command prompt.

```
Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.

WARNING: Re-reading the partition table failed with error 16: Device or resource busy.
The kernel still uses the old table. The new table will be used at
the next reboot or after you run partprobe(8) or kpartx(8)
Syncing disks.
```
The warning that follows the `w` simply says it will not take place until after a reboot. So reboot.

## Adding the space to the Logical Volume ##

After the computer comes back up, we can add the space to the logical volume by following these steps -

First, create a new physical volume using `pvcreate`

```
pvcreate /dev/sda3
  Physical volume "/dev/sda3" successfully created
```

Now add the physical volume to the volume group. Use `vgdisplay` to see the volume group information and find the correct name, then use `vgextend` to actually add it to the volume group.

```
vgdisplay
  --- Volume group ---
  VG Name               centos_mpls-sv-nasfs02
```

Note that the Volume Group name here is centos_mpls-sv-nasfs02.  It can be anything, but this is the name you need.

```
vgextend centos_mpls-sv-nasfs02 /dev/sda3
  Volume group "mpls-sv-nasfs02" successfully extended
```

We can also confirm it is part of the Volume Group by running `pvscan`

```
pvscan
  PV /dev/sda2   VG centos_mpls-sv-nasfs02   lvm2 [<39.00 GiB / 4.00 MiB free]
  PV /dev/sda3   VG centos_mpls-sv-nasfs02   lvm2 [<60.00 GiB / 0    free]
```

As you can see, the second line returned shows /dev/sda3 is part of the VG group named centos_mpls-sv-nasfs02

The last part is to add it to the logical volume.  First we will look at the logical volume to confirm it's name, just as we did with the physical volume.

```
lvdisplay
  --- Logical volume ---
  LV Path                /dev/centos_mpls-sv-nasfs02/root
```

Typically, you will find 2 or 3 (and sometimes more) logical volumes in the display. They probably all look very similar but the last section will be different.
In this case, we are looking at the root volume in the /dev/centos_mpls-sv-nasfs02 volume group. There is also a swap and a temp there that I am not showing. We want to extend the root volume in this example, so that is the name we are looking for. Now just extend it using `lvextend`

```
lvextend /dev/centos_mpls-sv-nasfs02/root /dev/sda3
  Extending logical volume root to 28.90 GiB
  Logical volume root successfully resized
```

## Last Step - Recognizing the new space ##

The last part is where we just tell the OS that we have more space on that logical volume.  We do this by issuing the `xfs_growfs` command.

```
sfs_growfs /dev/centos_mpls-sv-nasfs02/root
```

Now you are done.  You can verify it using `df -h` and noting the newly added size to the disk space is now recognized.

---

[fdisk]:  images/Linux/fdisk.png "fdisk -l example"