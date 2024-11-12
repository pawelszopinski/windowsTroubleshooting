# Windows Troubleshooting Knowledge Base

This repository provides solutions for the most frequent Windows-related issues.

## Table of Contents
1. [Windows Update Issues](#windows-update-issues)
2. [Software Installation Errors](#software-installation-errors)
3. [Slow Performance](#slow-performance)
4. [Blue Screen of Death (BSOD)](#blue-screen-of-death-bsod)
5. [Internet Connectivity Issues](#internet-connectivity-issues)
6. [Managing Faulty Registry Entries](#managing-faulty-registry-entries)
7. [Creating a System Image Backup](#creating-a-system-image-backup)
8. [Restore a Computer after a Kernel Error](#restore-a-computer-after-a-kernel-error)

## Windows Update Issues
Many users face problems while updating their Windows OS. The update process might fail, or the PC may start behaving abnormally after an update.

**Solution:** Try running the Windows Update Troubleshooter. If it doesn't work, you can manually download and install the update from the Microsoft Update Catalog.

## Software Installation Errors
Sometimes, users might face issues while installing a new software or application on their Windows PC.

**Solution:** Ensure that your PC meets the minimum requirements of the software. If the problem persists, try to run the installation file as an administrator.

## Slow Performance
Over time, a Windows PC might start running slow due to various reasons like low disk space, too many startup programs, etc.

**Solution:** Perform a disk cleanup to free up space. Also, disable unnecessary startup programs from the Task Manager. 

## Blue Screen of Death (BSOD)
This is a common issue where the PC crashes and displays a blue screen with an error code.

**Solution:** The solution depends on the error code. Often, updating drivers or performing a system restore can solve the problem.

## Internet Connectivity Issues
Users might face problems while connecting their PC to the internet.

**Solution:** Try resetting your network settings. If the problem persists, update your network adapter drivers.



## Managing Faulty Registry Entries

1. **Initialize faulty entry**

- On the search bar, type: `regedit`
- Navigate to the following registry path: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services`.
- Click the `kbdclass` key to display the contents of this key on the details pane at the right.
- Right-click `Start` and select `Modify…`
- On the `Edit DWORD (32-bit) Value` dialog box, notice that the current value is set to 3.
- Change the `Value data` to 4
- Click `OK`.
- To enable the changes to take effect, you need to restart the system.

2. **Rollback the Registry Change Using Last Known Good Configuration**

- Press and hold the `F8` to invoke the Advanced Boot Options
- Select the `Troubleshoot` option on the Choose an option screen.
- Select the `Startup Settings` option.
- Click the `Restart` button to enable boot logging.
- Use your keyboard’s down arrow key to select the `Last Known Good Configuration (Advanced)` option and then press `Enter`.
- Verify the Effect of the Last Known Good Configuration Feature through e.g `regedit`

## Creating a System Image Backup

1. **Create a VHD**

- In Hyper-V vm settings On the Hardware pane at the left, click `SCSI Controller` node.
- Ensure that `Hard Drive` is selected as the drive to attach to the controller.
- Click `Add`.
- On the details pane, go to the Media section, then click `New`.
- On the `Choose Disk Type` page, verify that `Dynamically expanding` option is selected.
- On the `Specify Name and Location` page, type: `Disk2.vhdx`
- On the `Completing the New Virtual Hard Disk Wizard` page, click `Finish` to create the new disk.
- Click `OK` to save the changes and exit the console.

2. **Initialize and Format a VHD**

- On the Server Manager Dashboard, click the Tools menu and select `Computer Management`.
- Expand the `Storage` node and then click `Disk Management`.
- Right-click the `Disk 1` label area and select `Online`.
- Right-click the `Disk 1` label area and select `Initialize Disk`.
- Right-click on `Disk 1’s Unallocated` section and choose `New Simple Volume…`
- On the `Assign Drive Letter or Path` page, keep the assigned drive letter as is.
- On the `Completing the New Simple Volume Wizard` page, click `Finish` to accept the specifications and format the disk.

3. **Install Windows Server Backup**

- The Windows Server Backup feature can be installed using either the graphical Add Roles or Features Wizard or Windows PowerShell commands.
- Administrator: Windows PowerShell window is displayed.
- To install Windows Server Backup, type: `Add-WindowsFeature Windows-Server-Backup -IncludeManagementTools` (last argument optional).
- Press `Enter`. Please wait while Windows Server Backup is being installed.

4. **Create a System Image Backup**

- The Windows Server Backup feature enables you to schedule regular backups or perform a one-time backup.
- On the menu bar at the top, click `Tools > Windows Server Backup`.
- Click the `Local Backup` node on the Windows Server Backup (Local) pane at the left.
- On the Actions pane at the right, click `Backup Once…`
- The Backup Once Wizard window is displayed.
- On the Backup Options page, accept default selections and click `Next`.
- On the Select Backup Configuration page, select the `Custom` option. Click `Next`.
- On the Select Items for Backup page, click `Add Items`.
- On the Select Items dialog box, select the `Bare metal recovery` check box. This will auto-select the other components, namely System state, EFI System Partition and Local Disk (C:) to backup. Click `OK`.
- On the Specify Destination Type page, ensure that `Local drives` radio button is selected. Click `Next`.
- On the Select Backup Destination page, the New Volume (E:) is automatically selected because that is the only secondary storage available on the system. Click `Next`.
- On the Confirmation page, read the summary of specifications to create this backup. Click the `Backup` button to create backup.

## Restore a Computer after a Kernel Error

1. **Cause a BSOD Error**

- Type `regedit` to open the Registry Editor console.
- Navigate to the following registry path: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services`
- Click the `ACPI` node to display the contents of the folder on the details pane at the right.
- Locate `Start` on the details pane. Notice that the current data value for the Start parameter is set to 0.
- Right-click `Start` and select `Modify`.
- On the `Edit DWORD (32-bit) Value` dialog box, type `5` in the `Value data` text box.
- Click `OK` to save changes and exit the dialog box.
- Restart the system to enable the changed settings to take effect. Windows Server will continue to load the device drivers.
- You will encounter a blue screen of death (BSOD) because the ACPI device driver startup value was changed to an incorrect value. Windows will restart and automatically enter into the repair mode.

2. **Restore the System**

- On the `Choose an option` screen, select the `Troubleshoot` option.
- On the `Advanced options` screen, select the `System Image Recovery` option.
- Once the login credentials for the user account are verified, the `Re-image your computer` wizard appears.
- On the `Select a system image backup` page, select the most recent system image and click `Next`.
- On the `Choose additional restore options` page, do not select any options (as there are no particular restoration settings for this task) and click `Next`.
- Review the additional information about the system image backup that you are about to restore, then click `Finish`.
