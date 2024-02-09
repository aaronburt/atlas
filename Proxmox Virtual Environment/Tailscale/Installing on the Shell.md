
First [download](obsidian://open?vault=Atlas&file=Proxmox%20Virtual%20Environment%2FTailscale%2FDownloading)


Installing on the shell requires a modified version of the `up` command. This prevents Tailscale from overwriting your DNS, which could render LXC's and VM's using `HOST DNS Settings` unable to resolve the `100.100.100.100` magic DNS.

The common workaround at this point is ONLY on the shell to run.
```bash
tailscale up --accept-dns=false
```

This enables Tailscale for connections while rejecting the use of its internal DNS. Keep in mind that magic DNS naming won't function, requiring you to specify IP addresses instead of machine names on the shell.