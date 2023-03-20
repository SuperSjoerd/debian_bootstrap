# Default debian setup steps

## Install vim

```bash
apt update && apt install -y vim
```

## Create non-root user

```bash
adduser sjoerd
usermod -aG sudo sjoerd
su - sjoerd
```

## Setup SSH

```bash
mkdir .ssh && vim .ssh/authorized_keys
chmod -R 700 .ssh
chmod 644 .ssh/*
```

## Disable non-root ssh

```bash
sudo vim /etc/ssh/sshd_config
> PermitRootLogin no
sudo systemctl restart sshd
```

## Install docker

```bash
sudo apt -y install apt-transport-https ca-certificates curl gnupg2 software-properties-common
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/docker-archive-keyring.gpg
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
sudo apt update && sudo apt install -y docker-ce docker-ce-cli containerd.io
sudo systemctl enable --now docker
sudo usermod -aG docker ${USER}
su - ${USER}
```

## Setup compose

```bash
mkdir -p ~/.docker/cli-plugins/
curl -SL https://github.com/docker/compose/releases/download/v2.16.0/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
chmod +x ~/.docker/cli-plugins/docker-compose
```

## Fix apparmor error (debian 11) (should be fixed by Docker?)

```bash
sudo apt install -y apparmor
sudo systemctl restart docker
```
