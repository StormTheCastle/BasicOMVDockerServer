# Calibre-web Container

_Required:_ Make sure you know:
- Your calibre user's PUID
- Your associated group GID (or calibre user PGID)
- Your timezone marker
- Your appdata absolute filepath (from Storage > Shared Folders > Absolute Path column)
- Your dockerdata absolute filepath (from Storage > Shared Folders > Absolute Path column)

## Compose File
- Services > Compose > Add > Add from Example > Find calibre-web
- This directory has an example compose file as well
- Edit the compose file:
  - Environment values:
    - Plug in your user id under PUID
    - Plug in your group id under PGID
    - Plug in your time zone under TZ
  - The volumes data paths might work as-is but I prefer to update to the absolute path. The left-hand side should be the path as docker sees it and the right-hand side will be how jellyfin can see it.
  - **Note:** To make sure I have access to the folder data, I created the calibre folder in /dockerdata
    - Ex. I had a path to /dockerdata/books:/books. That way the container would find it automatically and not try to create its own.
- Save
- Click on the new line in the Services > Compose > Files table and click the "Pull" icon
- Click "Up" after "Pull" is done

## Test
- Use the local IP (or the tailscale IP if you have it up), and append :8083 in your browser search bar and launch
- The web gui should launch
- The default login is admin/admin123. Use that to log in and edit the user name/password to what you want.
- You can now move your files into the folder the library points to and the GUI should find them
- If it doesn't, check the user/group security for the folders and the ones you plugged into the compose file
