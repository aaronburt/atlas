
### Goal 

Your goal is to share local services like a [Homepage](https://gethomepage.dev) but you don't want to use Cloudflare Tunnels or expose you personal network IP address to the internet. You might be also be unable to do so as your network is inside a [CGNAT](https://en.wikipedia.org/wiki/Carrier-grade_NAT)

### Requirements

You will need the following in order to follow this guide. 

* Affordable VPS with root access 
	* Recommend Oracle for free 
	* Scaleway for cheap European based
	* Google Cloud for secure
* Tailscale account
* Local machine that will host the application in question

### The idea 

The idea is that the VPS will host a [Reverse Proxy](https://www.cloudflare.com/en-gb/learning/cdn/glossary/reverse-proxy/) called [Nginx Proxy Manager](https://nginxproxymanager.com/) and will talk via an Tailscale Wireguard encrypted tunnel to a local machine on your personal network that will host the application in question. This means important credentials don't need to be saved on the VPS and your public IPV4 address doesn't need to be open to the elements. 

### The Execution 

First you will need to install Tailscale on your local machine and login either on Linux by using `tailscale up` command or on Windows by using the application. 

Next SSH into the VPS in question and again install [Download](https://tailscale.com/download/linux) Tailscale and run `Tailscale up` to login. Next verify that the Tailscale client has established connection successfully to the Coordination server (Ran by Tailscale) and is able to 'See' the other device. 

Easiest way to check the devices are `Online` is by going to the Coordination service's admin panel [here](https://login.tailscale.com/admin/machines). You should see a List of `Machines` with the Machine, Addresses, Version and Last Seen. Take a note of the machine names, they normally try to follow the `Hostname` of the device they came from.  If the last seen says Connected then its all good as far as the Coordination server is concerned. 

Following on from that we need to verify that the VPS can talk to the Local machine. To do this in either the VPS or the Local machine open a terminal window and run
``
```bash
tailscale status
```

It should list all the machines its ACL policies allow it to see. 

```bash
100.111.13.102  ct-adguard           tagged-devices linux   active; direct 192.168.1.249:41641, tx 306732 rx 467076
```