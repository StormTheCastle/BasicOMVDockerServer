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
This is to create the folders docker uses for data access, etc. Storage > Shared Folders. You will create a minimum of four folders. The name can be whatever you want, but These will be used to store docker-specific data, so good to name prepended with "docker" and they will be plugged into Compose > Settings later.

- dockerappconfig (or "appdata") - for docker specific config and persistent data for containers. Container config data.
  - permissions: admin read/write | Users no access | Others no access
  - file system - choose whichever
- dockercomposeconfig (or "docker") - for docker specific config. The files that docker uses internally, there is no need to preserve the data in this folder
  - permissions: admin read/write | Users no access | Others no access
  - file system - choose whichever
- dockerbackup (or "backup_compose") - backups for persistent data
  - permissions: admin read/write | Users read/write | Others read only
  - file system - the larger/main data system disk
- dockerdata  (or "data") - main data of the containers (ex. the music/movies)
  - permissions: admin read/write | Users read/write | Others read only
  - file system - the larger/main data system disk
  - _Note:_ You can split this up further with subfolders for data access per user. For example, I made a dockerdata/media folder for jellyfin and dockerdata/books for calibre.


## Users and Groups
