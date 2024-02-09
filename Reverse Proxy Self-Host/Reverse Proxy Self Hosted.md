
You want to host a website or a server but don't want/can't open ports on your local connection and don't want to expose your IP address. This can be done in numerous ways, some are far more simple than others but today I will describe how  to setup a Nginx Proxy Manager on a VPS to your local machine.  

### Rejecting Cloudflare Tunnels

My Rational for rejecting Cloudflare Tunnels is a rather simple, I'm paranoid. But seriously if you think about it you do leave a potential door wide open on your network by introducing Cloudflare tunnels into the equation. This program does have access to all local IP addresses on a traditional home network (Excluding the OPNSense types).

### Introducing Tailscale

Tailscale is a VPN solution for peer to peer communication built on top of the more well known Wireguard which is slick, fast and most importantly very secure. If you want to take the security to the next step you could even remove their coordination server and instead use `headscale`.

But for basic steps we will use the standard Tailscale. 

### VPS Setup 

Setting up the VPS is simple with most cloud services. Go onto their platform and rent a box, this box will need to have access to a recent version of Linux and needs root.

