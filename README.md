#### orginal by  printfuck

this is the unircornsrange server 

# Stardew Valley Multiplayer Docker Compose

This project aims to autostart a Stardew Valley Multiplayer Server as easy as possible.

## Setup

### Docker-Compose
 
```
git clone https://github.com/printfuck/stardew-multiplayer-docker

docker-compose up
```
### Ansible

Create an inventory file with your hosts

```
ansible -i <your_inventori> playbook.yml
```

### Terraform (with Hetzner Cloud)

Enter your API Token in `terraform/vars.auto.tfvars` and modify the resource section in `main.tf` to your liking, then run the following script:

```
./terra.sh
```

## Game Setup

Intially you have to create or load a game once at first startup via VNC. After that the Autoload Mod jumps into the previously loaded savegame everytime you rerun the container. You can also edit the config file of the Autoload Mod to archieve similar behaviour.

### VNC

Use a vnc client like `TightVNC` on Windows or plain `vncviewer` on any Linux distribution to connect to the server. You can modify the VNC Port and IP address and Password in the `docker-compose.yml` file like this:

Localhost:
```
   # Server is only reachable on localhost on port 2342...
   ports:
     - 127.0.0.1:2342:5900
   # ... with the password "insecure"
   environment:
     - VNCPASS=insecure
```

## How it works

The game, the modloader (SMAPI), and the necessary Mods are pulled from my servers (I'll assume you already own the game - since you're looking for a multiplayer - so please don't rip it from there) to minimize version conflicts. The `docker-entrypoint.sh` script will start `Xvfb` and `x11vnc` before starting the game. You can control the game via vnc with the settings within the `docker-compose.yml` file.

## Used Mods

* [AutoLoadGame](https://www.nexusmods.com/stardewvalley/mods/2509)
* [Always On](https://community.playstarbound.com/threads/updating-mods-for-stardew-valley-1-4.156000/page-20#post-3353880)
* [Unlimited Players](https://www.nexusmods.com/stardewvalley/mods/2213)

## Troubleshooting

### Error Messages in Console

Usually you should be able to ignore any message there. If the game doesn't start or any errors appear, you should look for messages like "cannot open display", which would most likely indicate permission errors.

### VNC

Access the game via VNC to initially load or start a pregenerated savegame. You can control the Server from there or edit the config.json files in the configs folder.

### Performance

I'd recomend a VPS/Machine with at least four logical CPUs and 4GB Ram, otherwise there will be horrible lags. The minimum configuration I'd consider playable with two to four players would be two logical CPUs and 1GB of Ram.
