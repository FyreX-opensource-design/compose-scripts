# Immich docker + Caddy for domain

## requirements (for local hosting within network)

* Tailscale account
* NextDNS account
* debain 12 host or VM (LXC's don't work)

## VM requirements
* As much storage as you can spare (single drive) or at least 16-32 GB (seperate drive storage)
* 6 Threads (CPUs in proxmox)
* 6 GB of RAM

# instructions
1. Setup a debain 12 VM in proxmox with a sudo enabled account. Don't use an LXC, Immich doesn't behave in them
2. run all these commands to install docker compose v2.0
```bash
sudo apt update
sudo apt install -y ca-certificates curl gnupg lsb-release
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/debian $(lsb_release -cs) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
3.  install the packages `docker-compose-plugin`, `git`, and `caddy`.
4.  Clone this repo branch
5.  edit the domain to use in `.env` and `Caddyfile`. Replace every instance of `immich.home` with it. don't use .local TLD.
6.  move the Cadddyfile to /etc/caddy/Caddyfile
7.  install tailscale on the server via `curl -fsSL https://tailscale.com/install.sh | sh` and log it into your tailnet. You might have to use ssh to paste the command into a proxmox VM
8.  Assuming you have a NextDNS account setup and it being used for your tailnet's DNS, go to Settings -> Rewrites under your main profile and add your chosen domain as a rewrite to the Immich server's tailscale IP
