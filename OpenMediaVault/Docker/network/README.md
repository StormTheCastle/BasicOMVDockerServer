# Pihole (and nginx-proxy-manager) Container

_Required:_ Make sure you know:
- Your network/pihole/app user 
- Your data absolute filepath (from Storage > Shared Folders > Absolute Path column)
- The name of your VLAN network (setup described below)

_Required:_
- Assign a static address for your pi through your router as was recommended during initial pi setup

## VLAN Setup
Wiki reference [here](https://wiki.omv-extras.org/doku.php?id=omv7:omv7_plugins:docker_compose#how_to_create_a_vlan_pi-hole_adguard)

Prep:
- On your router, you will want to take note of available IP values for use on a VLAN. For example:
  - My router DHCP auto assignment IP address range was set from 192.168.0.100 to 192.168.0.199
  - The full range of addresses should be from 0 - 255 for the end
  - I decide I will manually assign IP addresses out of 0-100 (except .1 which is reserved for the router gateway)
  - I make a note that 200-255 will be for VLAN from the docker
 
Create VLAN Network:
- Go to Services > Compose > Networks
- Create a new network
  - Name: choose a name (ex. pihole-network) and remember it
  - Driver: macvlan
  - Subnet: subnet available range (ex. 192.168.0.0/24)
  - Gateway: <Your gateway IP>
  - IP range: ip range within the subnet to be reserved for the pihole setup. (ex. I chose 192.168.0.201/32 so it's always going to be .0.201)
  - Note: you can use this [ip calculator](https://jodies.de/ipcalc) to help

## Changing Ports
- If you want to use pihole to do local domain name stuff, you will also need to adjust some ports
- Go to OpenMediaVault login and go to System > Workbench
  - Change the port to something like 82
- (Opt.) Check SSL/TLS enabled
  - Change port to 444
  - Make sure your certificate isn't too long
- The pihole port will be changed below as well

## Pi-Hole Compose File
- Services > Compose > Add > Add from Example > Find pihole
- This directory has an example compose file as well
- Edit the compose file:
  - Add the networks interface following the example
  - Environment values:
    - Plug in your user/group/timezone/web password
  - Adjust the ports for 80 and 443 to be different unshared values
  - The volumes data paths might work as-is but I prefer to update to the absolute path. The left-hand side should be the path as docker sees it and the right-hand side will be how the container can see it.
- Save
- Click on the new line in the Services > Compose > Files table and click the "Pull" icon
- Click "Up" after "Pull" is done

## Note
Do not attempt to upgrade (pihole -up) or reconfigure (pihole -r).
New images will be released for upgrades. Upgrading by replacing your old container with a fresh upgraded image is the 'docker way'. Long-living docker containers are not the docker way since they aim to be portable and reproducible, why not re-create them often! Just to prove you can.

## Nginx-proxy-manager Compose File
Used for rerouting domains - helpful to have text addresses for the various containers we're running.

- Services > Compose > Add > Add from Example > Find nginx-proxy-manager
- This directory has an example compose file as well
- Edit the compose file:
  - Add the networks interface following the example
  - Environment values:
    - Plug in your user/group/timezone
  - You might be able to adjust the 80/443 ports but since we already changed OMV and pi-hole, you shouldn't need to here
  - The volumes data paths might work as-is but I prefer to update to the absolute path. The left-hand side should be the path as docker sees it and the right-hand side will be how the container can see it.
- Save
- Click on the new line in the Services > Compose > Files table and click the "Pull" icon
- Click "Up" after "Pull" is done
- This takes a little more time to spin up, so wait a few minutes for it to launch

## Pi-hole web GUI access
- Use the IP you set for the VLAN as noted in the pihole compose file in your browser search bar and launch
- The web gui should launch (it might redirect you to the <ip>/admin/login)
- Use the password you input in the compose file and log in

## Nginx web GUI access
- This should be accessible via your pi IP port 81 (ie. 192.168.X.X:81)
- admin@example.com / Password: changeme
- Change your credentials upon login

## Final Setup
- In your router, change your primary DNS to the pi-hole VLAN IP. You will probably have to reboot
- Check that you can still access everything correctly (test your internet and accessing the pi)

Pi-Hole GUI:
- You can also see under Tools > Tail log files > pihole.log and see various requests as they are routed through the pi-hole
- Add to block lists if you want - pi-hole can now help clear out more ads. A list of possible domains to add can be found in ad-block-lists.txt
- You can also do the reverse and add to allow-lists
- Click the toggle in the top right to switch to the expert/advanced mode if needed
- (Opt.) Go to Settings > Local DNS records
  - Under domain, add text domain labels for all of the addresses you want to be able to use and have them point to the pi IP (no port)
  - Ex. jellyfin.local | <pi IP>

Proxy Manager GUI:
- Hosts > Proxy Hosts > Add Proxy Host
- For each domain you listed in the pi-hole domain, add a new entry
  - set a domain name matching the label above (ex. jellyfin.local)
  - forward to the pi IP and the corresponding port (ex. jellyfin should be using 8096)
  - Save

## Test
- Type in the text domain and it should route to the actual local site (ex. jellyfin.local in the browser address bar should redirect to the jellyfin site you have)
