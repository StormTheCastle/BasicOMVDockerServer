# Pihole Container

_Required:_ Make sure you know:
- Your timezone marker
- Your network user 
- Your appdata absolute filepath (from Storage > Shared Folders > Absolute Path column)
- Your dockerdata absolute filepath (from Storage > Shared Folders > Absolute Path column)

_Required:_
- Assign a static address for your pihole through the router

## Changing Ports
- If you want to use pihole to do local domain name stuff, you will also need to adjust some ports
- Go to OpenMediaVault login and go to System > Workbench
  - Change the port to something like 82
- Check SSL/TLS enabled
  - Change port to 444
- The pihole port will be changed below

## Compose File
- Services > Compose > Add > Add from Example > Find pihole
- This directory has an example compose file as well
- Edit the compose file:
  - Environment values:
    - Plug in your time zone under TZ
  - The volumes data paths might work as-is but I prefer to update to the absolute path. The left-hand side should be the path as docker sees it and the right-hand side will be how the container can see it.
- Save
- Click on the new line in the Services > Compose > Files table and click the "Pull" icon
- Click "Up" after "Pull" is done

## Test
- Use the local IP (or the tailscale IP if you have it up), and append :<TODO> in your browser search bar and launch
- The web gui should launch
- TODO 
