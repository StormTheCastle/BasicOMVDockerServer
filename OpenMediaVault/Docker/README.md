# Docker Setup
This will cover setting up Docker and a few startup containers as described by the [OMV wiki](https://wiki.omv-extras.org/doku.php?id=omv7:docker_in_omv). This condenses steps for all of them, but can be skipped for solo instructions with the same steps exist in their subdirectories.

## Basic Setup
1. Install Docker: System > OMV-Extras > Enable Docker repo > Save
2. Install Plugins (System > Plugins > Search < Install):
   - **Docker**: openmediavault-compose: managing docker containers/resources
   - Optional, but may be useful:
     - openmediavault-scripts: running sripts in containers
     - openmediavault-resetperms: reset perms on a shared folder
		 - openmediavault-backup: backup for emergency
		 - openmediavault-clamav: antivirus
		 - openmediavault-ftp: ftp server
		 - openmediavault-fail2ban: scans logs and bans ip
		 - openmediavault-nut: emergency power supply backup monitoring system
		 - openmediavault-cputemp: cpu temp monitoring plugin
		 - (Not in OMV8 yet) openmediavault-diskclone: clone disk

## Data Folders
