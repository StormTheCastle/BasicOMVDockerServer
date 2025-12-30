# Tailscale Container

_Required:_ Make sure you know:
- Your pi's IP Address (you probably want to go to your router and make it a static value)
- Your dockerdata absolute filepath (from Storage > Shared Folders > Absolute Path column)
- Make sure your SSH user security includes the "docker" group (Users > Users)

## Tailscale Account
- You will need to have a tailscale account and an app key. If you already do, skip to the [Compose File](#Compose-File)
- Get tailscale key:
  - Go to Tailscale admin web gui
  - Settings > Personal > Keys > Generate auth key...
	  - Preapproved
	  - If playing around or if you think you'll want to re-use this, can make it re-usable and change expiration
	- Copy the key and store for following steps

## Compose File
- Services > Compose > Add > Add from Example > Find tailscale
- This directory has an example tailscale compose file as well
- Edit the compose file:
  - Environment values:
    - Plug in your key under TS_AUTHKEY
    - Change TS-ROUTES to actual IP beginning
  - CHANGE_TO_COMPOSE_DATA_PATH should work as-is but I prefer to update to the absolute path
	- Optionally adjust values like the container name (ex. tailscale-pi)
- Save
- Click on the new line in the Services > Compose > Files table and click the "Pull" icon
- Click "Up" after "Pull" is done
- During first-time setup, you should do the steps in the "Run" section below. Other times, just clicking the "Up" button at this point is enough

## Run
- SSH into pi and run the following command:
  - First time: `sudo docker exec -it tailscale tailscale up --advertise-exit-node`
  - Subsequent updates: `sudo docker exec -it tailscale tailscale up --reset --advertise-exit-node`
  
You can add --exit-node-allow-lan-access=true so if you are doing tailscale on computer and want to access to local

If 
