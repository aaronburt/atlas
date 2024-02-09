
Setting up Tailscale on the Proxmox Virtual Environment is very simple. It's essential to note that, from Tailscale's perspective, every LXC and VM is treated as a distinct device, impacting the overall device count. Each device comes with its Wireguard connection and private IP address, among other features. This is a viable alternative to Cloudflare tunnels for most private use cases. 

By default, Tailscale uses their coordination server. It helps devices know each other's IP addresses, public keys, and manages ACL policies. The connections are encrypted with Wireguard, so Tailscale can't peek at the content unless it needs to relay the traffic.

Alternatively, you can host Tailscale with a personal `Headscale` coordination server, an unofficial fork. This way, you need to secure the server endpoint, but you cut out Tailscale entirely. It's a cool alternative to Cloudflare tunnels for most private needs.