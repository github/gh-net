## List of Linux dependencies by distro

### Ubuntu/Debian

#### Automatic setup

Add to `devcontainer.json`:

```jsonc
{
    "features": {
        "github-cli": "latest",
        "sshd": "latest"
    }
}
```

#### Manual setup

1. Sudo: `apt install sudo`

2. GitHub CLI: see https://github.com/cli/cli/blob/trunk/docs/install_linux.md#debian-ubuntu-linux-raspberry-pi-os-apt

3. OpenSSH server: `sudo apt install openssh-server && service ssh start`

### Alpine Linux

1. GitHub CLI: see https://github.com/cli/cli/blob/trunk/docs/install_linux.md#alpine-linux

### Arch Linux

1. Sudo: `pacman -S sudo`

2. GitHub CLI: see https://github.com/cli/cli/blob/trunk/docs/install_linux.md#arch-linux

3. SSH user group: `sudo groupadd ssh`

### CentOS

1. Sudo

```shell
dnf --disablerepo '*' --enablerepo=extras swap -y centos-linux-repos centos-stream-repos
dnf distro-sync
dnf install sudo
```

2. GitHub CLI: see https://github.com/cli/cli/blob/trunk/docs/install_linux.md#fedora-centos-red-hat-enterprise-linux-dnf

3. SSH user group: `sudo groupadd ssh`

### Fedora

1. GitHub CLI: https://github.com/cli/cli/blob/trunk/docs/install_linux.md#fedora-centos-red-hat-enterprise-linux-dnf

2. OpenSSH server & `ssh` user group

```shell
sudo dnf install openssh-server
sudo systemctl enable sshd.service
sudo groupadd ssh
sudo usermod -a -G ssh_keys,sshd,ssh root
```

### Kali Linux

1. Sudo `apt-get install sudo`

2. GitHub CLI:

```shell
pt-get install curl
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
sudo apt-get update
sudo apt-get install gh
```

3. OpenSSH server & `ssh` user group

```shell
sudo apt-get install openssh-server
sudo groupadd ssh
sudo groupadd sshd
sudo usermod -a -G ssh,sshd root
```

### Mint

1. GitHub CLI:

```shell
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
sudo apt update
sudo apt install gh
```

2. `ssh` user group

```shell
sudo groupadd ssh
sudo usermod -a -G ssh root
```

### OpenSUSE

1. Sudo: `zypper in sudo`

2. GitHub CLI: https://github.com/cli/cli/blob/trunk/docs/install_linux.md#opensusesuse-linux-zypper

3. `ssh` user group:

```
sudo groupadd ssh
sudo groupadd sshd
sudo usermod -a -G ssh,sshd root
```

### Red Hat Enterprise Linux

1. Sudo: `dnf install sudo`

2. GitHub CLI: https://github.com/cli/cli/blob/trunk/docs/install_linux.md#fedora-centos-red-hat-enterprise-linux-dnf

3. OpenSSH server & `ssh` user group

```shell
sudo dnf install openssh-server
sudo systemctl enable sshd.service
sudo groupadd ssh
sudo usermod -a -G ssh_keys,sshd,ssh root
```
