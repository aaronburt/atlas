
This serves as a guide for setting up Proxmox Virtual Environment (PVE), distinct from a similarly named mail gateway service. Proxmox enables the deployment of VMs and a lightweight alternative called LXC (Linux Containers) through a centralized web GUI accessible on port 8006.

After completing the standard installation, execute the Post-install script available [here](https://tteck.github.io/Proxmox/).

The script removes subscription-required repositories, replaces them with free alternatives, and updates all repositories, ensuring the latest versions. It also makes minor CSS tweaks to eliminate Proxmox 'reminders' for a subscription. Run this script in the **Proxmox shell via the WebUI**, as it can extract variables from the UI to aid in installation.

```
bash -c "$(wget -qLO - https://github.com/tteck/Proxmox/raw/main/misc/post-pve-install.sh)"
```

Next, apply the 'Discord Dark theme' install script to give the WebUI a clean, Discord-inspired dark theme. Again, install this via the shell:

```
bash <(curl -s https://raw.githubusercontent.com/Weilbyte/PVEDiscordDark/master/PVEDiscordDark.sh ) install
```

It's advisable to ensure a strong password, preferably at least 20 characters, to prevent remote attacks.

