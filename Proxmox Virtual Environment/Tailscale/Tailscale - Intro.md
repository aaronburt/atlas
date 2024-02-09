Setting up Tailscale on the Proxmox Virtual Environment is very simple. It's essential to note that, from Tailscale's perspective, every LXC and VM is treated as a distinct device, impacting the overall device count. Each device comes with its Wireguard connection and private IP address, among other features. This is a viable alternative to Cloudflare tunnels for most private use cases. 

By default, Tailscale uses their coordination server. It helps devices know each other's IP addresses, public keys, and manages ACL policies. The connections are encrypted with Wireguard, so Tailscale can't peek at the content even if it needs to relay the traffic.

Alternatively, you can host Tailscale with a personal `Headscale` coordination server, an unofficial fork. This way, you need to secure the server endpoint, but you cut out Tailscale entirely.

## Various methods of setup 

* [Installing with Proxmox Scripts](obsidian://open?vault=Atlas&file=Proxmox%20Virtual%20Environment%2FTailscale%2FInstalling%20with%20Proxmox%20Scripts)
* [Installing on LXC](obsidian://open?vault=Atlas&file=Proxmox%20Virtual%20Environment%2FTailscale%2FInstalling%20from%20LXC)
* [Installing on the Proxmox Shell](obsidian://open?vault=Atlas&file=Proxmox%20Virtual%20Environment%2FTailscale%2FInstalling%20on%20the%20Shell)
