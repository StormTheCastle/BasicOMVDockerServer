# Starting with OpenMediaVault
This follows along [OMV's new user guide](https://wiki.omv-extras.org/doku.php?id=omv7:new_user_guide#initial_configuration). We start off after the steps from the original [README](/README.md)

Reminder: You should have your pi's IP address (something like 192.168.XXX.XXX) and the OpenMediaVault default user and password (admin/openmediavault).

1. Launch the OMV web console and log in
2. Using the person icon in the top right, change the password to something more secure.
3. Go down through the System Settings.
   - System > Update Management > Settings > Software > Check Community-maintained updates then save
   - System > Update Management > Updates
     - Click the magnifying glass on the top left to check for new updates
     - Install all updates
   - Network > General > Make the domain name "internal" (or "local") and change hostname if desired
   - Optional:
     - System > Workbench: Automatic logout to 60 mins (for ease)
     - Date & Time
     - System > Power Management > power button behavior
   - Make sure you see omv-extras. If not, install it via SSH command `wget -O - https://github.com/OpenMediaVault-Plugin-Developers/packages/raw/master/install | bash`
    
## Storage
If you have extra storage disks you want to make available or want to add more after the fact:

1. Storage > Disks > Click on non-boot device(s) and "wipe" > quick
2. Storage > File Systems
   1. (Create) > EXT4 > Select the device > save
	 2. Save and apply
   3. Verify the available space looks good and take note of the device names and which disk it belongs to (ex. /dev/sda1 for your boot drive)
	 4. Repeat this for each of your additional storage devices
3. Storage > Shared Folders will be where you create the folder system of data. Docker follows an expected pattern so we will do this later in the [Docker Data Folders section](/OpenMediaVault/Docker/README.md)

### Monitoring
- Storage > S.M.A.R.T
	- Setting > Enable (Fill in whatever you need, then save)
	- Devices > Select the disk > Edit > Enable Monitoring > Save (for each device)

## Users and Groups
Users > Users/Groups will be used to control access. You can create one generic "appuser" with general access to data files to be shared by all the containers or you can make one for each. The steps here will have a user for each container and will be described in more detail in the [Docker Users and Groups section](/OpenMediaVault/Docker/README.md)
