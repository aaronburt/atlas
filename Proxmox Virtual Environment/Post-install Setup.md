
This is a Post-install setup guide for Proxmox Virtual Environment (PVE), not to be confused with the mail gateway service of a similar name. Proxmox allows deploy VM's and a lightweight alternative called LXC (Linux Containers) all from one centralised web GUI accessible on port 8006.

Once the standard installation has finished you first need to run this Post-install script from 
https://tteck.github.io/Proxmox/

It will remove subscription required repos and replace them with free alternatives, update and upgrade all repos ensuring you have the latest versions of everything. Edit some minor CSS tweaks to remove Proxmox 'reminders' for requiring a subscription. 

This MUST be ran in Proxmox shell via the WebUI as it can pull variables from the UI to help with installation. 

```
bash -c "$(wget -qLO - https://github.com/tteck/Proxmox/raw/main/misc/post-pve-install.sh)"
```

Next one is using the 'Discord Dark theme' install script to give the WebUI a clean discord inspired dark theme. 

Again must be install via the Shell
```
bash <(curl -s https://raw.githubusercontent.com/Weilbyte/PVEDiscordDark/master/PVEDiscordDark.sh ) install
```

Its also recommended that you ensure that your password is very strong, I recommend at-least 20 characters to prevent a remote attack. 

