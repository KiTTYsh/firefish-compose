# Description
Docker images and a docker-compose.yml that allow you to run [Firefish](https://joinfirefish.org/) with (almost) no configuration.  Just set the URL in docker.env and reverse proxy to port 3000!

The images for firefish and sonic also accept many environment variables allowing for easier setup/automation and better compatibility while using varied Docker frontends (Portainer/Unraid).

The images should *also* be acceptable for drop-in use with existing configurations where a .config volume is specified.

The docker-compose file uses inline named volumes.  IMO, this is really a matter of preference, so oh well if it doesn't suit you, but if you're just trying to get things working, it doesn't require any configuration and can be built up and torn down very quickly if you're trying to test things.

# Set up on Ubuntu 23.04
(Tested on a fresh Ubuntu 23.04 install on Linode as root)
```bash
# Configure your system to your specifications, change these as necessary
hostnamectl set-hostname firefish
timedatectl set-timezone America/New_York

# Update your system
apt update
apt upgrade -y # If you receive any prompts, press enter until done.

# Install Docker, per https://docs.docker.com/engine/install/ubuntu/
apt install -y ca-certificates curl gnupg # Should already be installed on Linode
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
chmod a+r /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" >> /etc/apt/sources.list.d/docker.list
apt update
apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin # May also prompt, just push enter

# Reboot your system for updates and docker to take effect
reboot

# Download this repo and cd into it
# Change the last part of the git line to name the folder differently, useful if you want to run multiple instances
git clone https://github.com/KiTTYsh/firefish-compose firefish
cd firefish
nano docker.env # change FIREFISH_URL, POSTGRES_PASSWORD, SONIC_PASSWORD
docker compose up
```
