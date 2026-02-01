# Syncthing Container

_Required:_ Make sure you know:
- Your container user's PUID
- Your associated group GID (or user PGID)
- Your timezone marker
- Your appdata absolute filepath (from Storage > Shared Folders > Absolute Path column)
- Your dockerdata absolute filepath (from Storage > Shared Folders > Absolute Path column)

## Syncthing Local Side
- Go to [syncthing.net](https://syncthing.net/) and download/install the version matching your machine
- You will be able to run syncthing and access the web gui via [http://127.0.0.1:8384/](http://127.0.0.1:8384/)

## Compose File
- Services > Compose > Add > Add from Example > Find syncthing
- This directory has an example compose file as well
- Edit the compose file:
  - Environment values:
    - Plug in your user id under PUID
    - Plug in your group id under PGID
    - Plug in your time zone under TZ
  - The volumes data paths might work as-is but I prefer to update to the absolute path. The left-hand side should be the path as docker sees it and the right-hand side will be how the container can see it.
  - **Note:** Depending on the volumes you make available, it may try to create its own folders that don't give your user permission to access. To prevent this, create them yourself manually through the file explorer.
    - Ex. I had a path to /dockerdata/syncdata:/syncdata. I just created the syncdata folder manually before running anything
- Save
- Click on the new line in the Services > Compose > Files table and click the "Pull" icon
- Click "Up" after "Pull" is done

## Access
- Use the local IP (or the tailscale IP if you have it up), and append :8384 in your browser search bar and launch
- The web gui for the pi server instance of syncthing should launch

## Syncthing Setup
- Launch the web gui for both the pi server instance and the local instance
- Go to Actions > Settings > General (starting tab)
  - Edit the device names for each to differentiate them
  - You can disable Anonymous Usage Reporting here if you want
  - Edit Folder Defaults
    - General tab
      - set your path to the folder(s) to sync
        - Ex. Local I set to a folder in my Docs
        - Ex. Pi Server I set to the /data path I created in the compose
    - File Versioning tab
      - Set to how you like. I did:
        - Simple File Versioning
        - Keep 5 Versions
    - Save
  - GUI tab
    - Adjust GUI username and password for both instances
  - Connections tab
    - Uncheck the boxes: Enable NAT traversal, Global Discovery, and Enable Relaying
    - Keep Local Discovery On
  - Save

- (Optional?) Go to Actions > Advanced > Options
  - Auto Upgrade Interval (hours) - set to 0
  - Crash Reporting Enabled - uncheck
  - Releases URL - clear out (to upgrade Syncthing, you will have to manually replace the executable and restart syncthing)
- Actions > Advanced > Devices
  - Allowed Networks - limit to <pi IP>.0.0/24 (Ex. 192.168.0.0/24)
- Actions > Advanced > Defaults > Default Device
  - Allowed Networks - Set to <pi IP>.0.0/24 (Ex. 192.168.0.0/24)

## Connecting Local and Server
- Add Remote Device
- You can either select the nearby device ID that shows up or plug in the ID of the other machine found via Actions > Show ID
- (Opt.) In the Sharing tab, you can check "Introducer" to automatically add devices from the Introducer to our device list (for mutually shared folders)
  - Ie. When adding a third machine, you can set "Introducer" when connecting to one of the other machines to get both
- (Opt.) In the Advanced tab, you can check "Untrusted" if you want all folders shared with this device to be protected by a password, such that all sent data is unreadable without the given password.
- Save

## Local File Share
- From the local machine that will be making the changes to save to the server, click "Add Folder"
- Set your Folder Path to the folder that should be synced
- Under Sharing, select the device to share to
- Under File Versioning, turn off File Versioning (you will be making the changes and don't need versions)
- (Opt.) Under the Advanced tab, you can decide whether the folder will be Send and Receive or just one of them
- Save
- In the server device, a new option should show up to accept the shared folder. Accept it and set the folder path
  - You can double-check the Sharing and File Versioning tabs, but we should be able to save as-is
  - (Opt.) Under the Advanced tab, you can decide whether the folder will be Send and Receive or just one of them

## Testing
- Add a file in one of the synced folders
- You should see it sync
