
Setting up Tailscale on the Proxmox Virtual Environment is very simple. It's essential to note that, from Tailscale's perspective, every LXC and VM is treated as a distinct device, impacting the overall device count. Each device comes with its Wireguard connection and private IP address, among other features. This is a viable alternative to Cloudflare tunnels for most private use cases. 

### Downloading

Begin by executing the command below to automatically add the correct repositories and install Tailscale binaries.

**Note:** Before proceeding, confirm the accuracy of this command by checking [here](https://tailscale.com/download) under the Linux section.

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

### Installing on the Shell

Installing on the shell requires a modified version of the `up` command. This prevents Tailscale from overwriting your DNS, which could render LXC's and VM's using `HOST DNS Settings` unable to resolve the `100.100.100.100` magic DNS.

The common workaround at this point is ONLY on the shell to run.
```bash
tailscale up --accept-dns=false
```

This enables Tailscale for connections while rejecting the use of its internal DNS. Keep in mind that magic DNS naming won't function, requiring you to specify IP addresses instead of machine names on the shell.

### Installing from LXC

LXC's have a specific requirement before running Tailscale: access to the Tunnel device `/dev/tun`, which unprivileged containers lack by default.

Identify the device ID (found next to the device name on the WebUI) and edit the LXC configuration file (e.g., `/etc/pve/lxc/110.conf`).

Add these two lines

```text
lxc.cgroup2.devices.allow: c 10:200 rwm 
lxc.mount.entry: /dev/net/tun dev/net/tun none bind,create=file
```

Reboot the LXC and execute:

```bash
tailscale up
```

It should work seamlessly with Magic DNS support.

### Installing with Proxmox Scripts

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