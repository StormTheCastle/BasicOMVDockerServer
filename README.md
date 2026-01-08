# BasicOMVDockerServer
Steps for setting up an OMV Server with basic functionality and documenting some of the gotchas that bothered me.

## Requirements
- Raspberry pi
- Data storage (preferred: SSD and 2+ of them)
  - One will be used for booting
  - It is best practice to have a second one for storing most of the data, but you can also store it all on the boot drive too
  - Opt. If you want to use the boot drive, make sure to format it as EXT4 prior to doing anything else. Without formatting it, the partition OMV is able to access seems to be limited
- Ethernet connection
- Secondary computer
 
## Initial Setup
- We will be using steps from the [OpenMediaVault Raspberry Pi Install wiki](https://wiki.omv-extras.org/doku.php?id=omv7:raspberry_pi_install) - this should be the source of truth

On your main computer:
- Install [RaspberryPi OS Imager](https://www.raspberrypi.com/software/)
- Install [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)

1. Clear/Format your boot drive (at this point, format as EXT4 especially if you want to be able to access all the space on it)
2. Run the RaspberryPi OS Imager on the boot drive and follow the steps.
   - OS: Make sure to select RaspberryPi OS *Lite* (no desktop).
   - Set hostname/user (not "admin")/password/and enable SSH
   - Make sure to not set up wi-fi network connection (this makes it easier to identify the ethernet connection to ssh into later on and some of the upgrades resets the network in ways that don't play well with wi-fi connections)
   - If you want to install OMV7, use the Legacy Lite version. OMV8 is out now and should work without the legacy and while following the steps though
3. You are now ready to plug the boot drive into the pi and power it up. Log into your router and identify the IP Address associated with it.
4. SSH into the pi using putty and the pi's ip address and the user you created
5. Run the following commands in order. Note: After each reboot, wait a bit and then you'll have to re-SSH in:
   1. `sudo apt update`
   2. `sudo apt upgrade -y`
   3. `sudo apt install lsb-release`
   4. `sudo reboot`
   5. `wget -O - https://github.com/OpenMediaVault-Plugin-Developers/installScript/raw/master/preinstall | sudo bash`
   6. `sudo reboot`
   7. `wget -O - https://github.com/OpenMediaVault-Plugin-Developers/installScript/raw/master/install | sudo bash`
      - Wait for this to finish and don't close Putty in the meantime
6. OpenMediaVault should now be accessible using the web GUI. Open the pi's IP address in your browser. The default user/password should be `admin/openmediavault`

## Next Steps
You can now move onto setting up OMV and Docker. OMV provides a [new user guide](https://wiki.omv-extras.org/doku.php?id=omv7:new_user_guide) and a [docker guide](https://wiki.omv-extras.org/doku.php?id=omv7:docker_in_omv) which I also summarize.
1. [Basic Start](OpenMediaVault/Basic/README.md)
2. [Docker](OpenMediaVault/Docker/README.md)
   - This will aggregate steps for setting up Docker along with tailscale, jellyfin, and calibre-web. These also will exist as solo sub-options if preferred.
