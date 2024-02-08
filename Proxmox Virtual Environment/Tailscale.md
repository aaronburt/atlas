
Installing Tailscale on Proxmox Virtual Environment is a fairly simple operation. One important note to remember is that from tailscale perspective, each LXC and VM is considered a separate device and will contribute to the overall device limit. Each device will also have its own Wireguard connection and private IP address among other things.   

### Downloading

If you want a secure Wireguard connection to your shell without needing to open ports or use Cloudflare tunnels it takes about 2 minutes. 

Firstly run the command below, to automatically add the correct repos and install tailscale binaries. 

BEFORE doing so, verify this is the correct command to execute on [here](https://tailscale.com/download) and look for the download for Linux. 
```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

### Installing on the Shell

For installing on the shell you need to run an altered version of the `up` command, this is because otherwise tailscale will overwrite your DNS resulting in all LXC's and VM's that use `HOST DNS Settings` from being unable to resolve the `100.100.100.100` magic DNS as they technically won't have access to the tailscale. 

The common workaround at this point is ONLY on the shell to run.
```bash
tailscale up --accept-dns=false
```

Which will enable tailscale for connections but refuse to use its internal DNS, this does mean that the magic DNS naming won't work, which in affect means you will need to specify the IP address of the machines you want to connect to instead of using their machine name on the shell.

### Installing from LXC

LXC's have a special requirement before being able to run tailscale. They need access to the Tunnel device `/dev/tun` which by default unprivileged containers don't. 

To do this you need to know the ID of the device, in Proxmox this is next to the device name on the WebUI like this `tailscale-test-container (110)`. Once you know the device you can go into the shell and edit configuration file for the LXC which in my case would be `/etc/pve/lxc/110.conf`.

Add these two lines

```text
lxc.cgroup2.devices.allow: c 10:200 rwm 
lxc.mount.entry: /dev/net/tun dev/net/tun none bind,create=file
```

You then need to reboot the LXC and 

```bash
tailscale up
```

It should then work out of the box with the Magic DNS support. 

### Installing with Proxmox Scripts

If you don't want to do this yourself and would instead much prefer a simple wizard install use Proxmox scripts' install for Tailscale onto an LXC

```bash
bash -c "$(wget -qLO - https://github.com/tteck/Proxmox/raw/main/misc/add-tailscale-lxc.sh)"
```

After the script finishes, reboot the LXC then run `tailscale up` in the LXC console

### Installing on a VM

VM install is just a simple as

```bash
tailscale up
```