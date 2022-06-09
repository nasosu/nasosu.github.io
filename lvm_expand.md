# Extending a logical volume in a virtual machine running Red Hat or Cent OS

To extend the logical volume:
 
1. Power off the virtual machine.
2. Edit the virtual machine settings and extend the virtual disk size. For more information, see Increasing the size of a virtual disk (1004047).
3. Power on the virtual machine.
4. Identify the device name, which is by default /dev/sda, and confirm the new size by running the command:

`# fdisk -l`
 
5. Create a new primary partition:
  5.1. Run the command:

  `# fdisk /dev/sda (depending the results of the step 4)`
    
  5.2. Press `p` to print the partition table to identify the number of partitions. By default, there are 2: sda1 and sda2.
  5.3. Press `n` to create a new primary partition.
  5.4. Press `p` for primary.
  5.5. Press `3` for the partition number, depending on the output of the partition table print.
  5.6. Press `Enter` two times.
  5.7. Press `t` to change the system's partition ID.
  5.8. Press `3` to select the newly creation partition.
  5.9. Type `8e` to change the Hex Code of the partition for Linux LVM.
  5.10. Press `w` to write the changes to the partition table.
 
6. Restart the virtual machine.
7. Run this command to verify that the changes were saved to the partition table and that the new partition has an 8e type:

`# fdisk -l`
 
8. Run this command to convert the new partition to a physical volume:

Note: The number for the sda can change depending on system setup. Use the sda number that was created in step 5.

`# pvcreate /dev/sda3`
 
9. Run this command to extend the physical volume:

Note: To determine which volume group to extend, use the command `vgdisplay`.

Note: Additionally, for the remainder of the commands, VolGroup00 will be unique to each Guest and should be adjusted to reflect your specific VM.

`# vgextend VolGroup00 /dev/sda3`
 
10. Run this command to verify how many physical extents are available to the Volume Group:

`# vgdisplay VolGroup00 | grep "Free"`
 
11. Run the following command to extend the Logical Volume:

Note: To determine which logical volume to extend, use the command lvdisplay.

`# lvextend -L+#G /dev/VolGroup00/LogVol00`

Where `#` is the number of Free space in GB available as per the previous command. Use the full number output from Step 10 including any decimals.
 
12. Run the following command to expand the ext3 filesystem online, inside of the Logical Volume:

Note:
Use `resize2fs` instead of `ext2online` for non-Red Hat virtual machines.
Use `xfs_growfs` for Red Hat, CentOS 7 and other VM Guest OS types that use the XFS file system.
`# ext2online /dev/VolGroup00/LogVol00`
 
13. Run the following command to verify that the / filesystem has the new space available:

`# df -h /`
