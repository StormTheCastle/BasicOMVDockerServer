# Tailscale Container

_Required:_ Make sure you know:
- Your pi's IP Address (you probably want to go to your router and make it a static value)
- Your dockerdata absolute filepath (from Storage > Shared Folders > Absolute Path column)

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
