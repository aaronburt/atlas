
For those who prefer a hassle-free installation, utilize Proxmox scripts' Tailscale installation for an LXC:

```bash
bash -c "$(wget -qLO - https://github.com/tteck/Proxmox/raw/main/misc/add-tailscale-lxc.sh)"
```

After the script completes, reboot the LXC, then run `tailscale up` in the LXC console.

### Installing on a VM

VM installation is as straightforward as running:

```bash
tailscale up
```