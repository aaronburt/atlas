
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

First you will need to insta