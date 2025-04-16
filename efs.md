# EFS Setup Steps

Based on the document, here are the exact steps for setting up Amazon Elastic File System (EFS) across multiple instances:

## Creating Instances and Security Groups

1. Create the first EC2 instance (EFS-1) with default settings
2. Edit Network Settings:
   - Select a subnet
   - Create a new security group with a name and description
   - Add security group rule for NFS (port 2049) with source 0.0.0.0/0
3. Launch the instance

4. Create a second EC2 instance (EFS-2)
5. In Network Settings, select the existing security group created for the first instance
6. Launch the instance

## Preparing Instances for EFS

7. Connect to both EFS-1 and EFS-2 instances using SSH/Putty
8. On both instances, run these commands:
   ```
   sudo su
   mkdir efs
   yum install -y amazon-efs-utils
   ```

## Creating and Mounting EFS

9. Create a new EFS file system in the AWS console
10. Go to the network settings of EFS and click on "Manage"
11. Select the security groups for the particular regions and click "Save" then "Attach"
12. Copy the mount command provided in the console
13. Paste and run the mount command in both instances (the command should look something like):
   ```
   sudo mount -t efs fs-xxxxxxxx:/ efs
   ```

## Testing EFS Shared Storage

14. On EFS-1 instance, run:
   ```
   cd efs
   touch file1.txt
   ```
15. On EFS-2 instance, verify the file is visible (showing that storage is shared)

This completes the setup of an EFS file system shared between two EC2 instances.