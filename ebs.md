# EBS Step-by-Step Guide

Here's a comprehensive guide to working with EBS volumes on EC2 instances, extracted from your document:

## Creating an EC2 Instance with Additional EBS Volume

1. Create an EC2 instance as usual
2. Select instance type as t2.micro
3. Configure storage by adding a new volume during the creation process

## Adding Storage After Creating an Instance

1. Go to Volumes in the EC2 console
2. Create a volume (ensure it's in the same region as your instance)
3. Select the volume → Actions → Attach volume → Select your instance
4. Connect to your instance and run `lsblk` to verify the attachment

## Attaching and Mounting an EBS Volume

1. Connect to your instance
2. Create a partition on the new disk using fdisk:
   ```
   sudo fdisk /dev/xvdc
   ```
   (Follow the prompts to create a primary partition and write changes with 'w')
3. Refresh the system partition table
4. Create a mount point:
   ```
   sudo mkdir -p /mnt/cclab
   ```
5. Format the partition with XFS:
   ```
   sudo mkfs -t xfs /dev/xvdc1
   ```
6. Mount the partition:
   ```
   sudo mount /dev/xvdc1 /mnt/cclab
   ```
7. Verify with:
   ```
   lsblk -fs
   ```

## Making the Mount Persistent

1. Create and attach your volume (as described above)
2. Connect to your instance and check if the volume is detected:
   ```
   lsblk
   ```
3. Check if the volume already has a filesystem:
   ```
   sudo file -s /dev/xvdb
   ```
   (If it shows "data", the volume is unformatted)
4. Format the volume:
   ```
   sudo mkfs -t xfs /dev/xvdb
   ```
5. Create a mount point:
   ```
   sudo mkdir -p /mnt/cc
   ```
6. Mount the volume:
   ```
   sudo mount /dev/xvdb /mnt/cc
   ```
7. Verify the mount:
   ```
   df -h
   ```
8. Get the UUID of the volume:
   ```
   sudo blkid /dev/xvdb
   ```
9. Edit the fstab file:
   ```
   sudo nano /etc/fstab
   ```
10. Add this line at the bottom (replace with your UUID):
    ```
    UUID="d43e73ac-0aff-4c62-8f92-501502f326b6" /mnt/cc xfs defaults,nofail 0 2
    ```
11. Test the fstab entry:
    ```
    sudo mount -a
    ```
12. Reboot the instance and verify that the volume is still mounted:
    ```
    df -h
    ```

## Detaching a Volume

1. Check if the volume is mounted:
   ```
   df -h
   ```
2. Unmount the volume:
   ```
   sudo umount /mnt/cc
   ```
3. Verify it's unmounted
4. Go to your volume in the EC2 console
5. Select Actions → Detach volume → Confirm detachment

These commands will help you manage the full lifecycle of EBS volumes with your EC2 instances.