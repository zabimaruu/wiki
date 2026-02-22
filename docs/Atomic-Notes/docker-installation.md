2024-07-31 12:24

Tags: [[Docker]] [[Cross Platform]] [[CLI]]

# Table Of Content

## Installation

### Arch
Install `docker` and `docker-compose`
```bash
sudo pacman -S docker docker-compose
```

Start `docker` service
```bash
sudo systemctl enable docker.service
sudo systemctl start docker.service
```

### Debian
For more [up to date installation method visit docker main site](https://docs.docker.com/engine/install/debian/)
Uninstall any/all conflicted packages
```bash
sudo apt remove $(dpkg --get-selections docker.io docker-compose docker-doc podman-docker containerd runc | cut -f1)
```

Setup `docker` official repository
```bash
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/debian
Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```

Install `docker` by running
```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Check if `docker` is runnint
```bash
sudo systemctl status docker
```
If `docker` is disable, enable it
```bash
sudo systemctl start docker
```

Verify `docker` works properly, install a test image, by running
```bash
sudo docker run hello-world
```