# Syncthing Container

TODO: under construction

_Required:_ Make sure you know:
- Your container user's PUID
- Your associated group GID (or user PGID)
- Your timezone marker
- Your appdata absolute filepath (from Storage > Shared Folders > Absolute Path column)
- Your dockerdata absolute filepath (from Storage > Shared Folders > Absolute Path column)

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
- The web gui should launch

## Syncthing Setup
- TODO
