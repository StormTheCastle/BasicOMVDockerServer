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
  - _Note:_ You can split this up further with subfolders for data access per user. For example, I made a dockerdata/media folder for jellyfin and dockerdata/books for calibre. After the following two sections, you should also be able to access the file system from the file explorer and create subfolders in that way too. We will want to do that later as described in future steps.

## Docker Compose Settings
Services > Compose > Settings

1. Shared Folder: `dockerappconfig` (appdata)
```
Owner: root | group: root
permissions: admin read/write | users/others: no access
```
2. Data Shared Folder: `dockerdata` (data)
3. Backup Shared Folder: `dockerbackup` (backup_compose)
4. Docker Config Shared Folder: `dockercomposeconfig` (docker)
5. Save and apply
6. Return to Services > Compose > Settings and click the Reinstall Docker button on the bottom

## Users and Groups
Used for determining access. Users > Users/Groups. For each of your containers/services it's good practice to create a new user for them (or a group if needed).

_Note:_ Before starting, click the little grid/table by the search and make sure the UID/GID are visible. Keep track of user UID and group GID. (If you aren't doing anything with groups, the default "users" GUID should be used, which will probably be "100")
	
1. Create the users and groups and assign the correct permissions. Ex. Create a user "jellyfin"
   - Add password
   - Shell: /usr/sbin/nologin
   - Groups: Users
   - Disallow account modification
   - Save/Apply
2. Repeat the above for however many users you need.
3. For each, click on row and click the folder/"shared folder permissions" and allow read/write access to the shared folders they should be allowed in.
(This can also be done in Storage > Shared Folders and clicking the folder/"shared folder permissions")
   - Usually dockerappconfig and dockerdata (or the subfolder)

# Manual File Access
This will allow using the IP address and Windows file explorer network location traversal to view and edit files.

1. Services > SMB/CIFS
   - Settings: enable sharing
   - Shares: add lines for each folder to share and change to Inherit Permissions
2. Verify that typing in the Windows explorer bar (or using right-click This PC > Add network location) lets you access `\\<IP Address>\dockerdata` and that you may add folders and data directly to the drive.
   - If you have trouble with this, check that your user has the appropriate security for the folders.

At this point, you are ready to add compose containers. The basic steps will be: Services > Compose > Add / Add from Example > Fill out paths and variables > (Pull only first time setup or update) / Up.

Before you start, make sure you go to your shared files and keep track of the absolute paths of the various folders you will be referencing for data.

In my examples, for instance, appdata will be referred to using /dockerappconfig and the data folder will look like /srv/dev-disk-by-uuid-***/dockerdata

More detailed steps per container below:
- [Tailscale](/OpenMediaVault/Docker/Tailscale/README.md)
- [Jellyfin](/OpenMediaVault/Docker/Jellyfin/README.md)
- [Calibre-web](/OpenMediaVault/Docker/Calibre-web/README.md)
- [Pihole](/OpenMediaVault/Docker/Pihole/README.md)
