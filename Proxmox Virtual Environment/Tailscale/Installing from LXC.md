First [download](obsidian://open?vault=Atlas&file=Proxmox%20Virtual%20Environment%2FTailscale%2FDownloading)

LXC's have a specific requirement before running Tailscale: access to the Tunnel device `/dev/tun`, which unprivileged containers lack by default.

Identify the device ID (found next to the device name on the WebUI) and edit the LXC configuration file (e.g., `/etc/pve/lxc/110.conf`) inside the shell.

Add these two lines

```text
lxc.cgroup2.devices.allow: c 10:200 rwm 
lxc.mount.entry: /dev/net/tun dev/net/tun none bind,create=file
```

Next enter and reboot the LXC then execute:

```bash
tailscale up
```

It should work seamlessly with Magic DNS support.