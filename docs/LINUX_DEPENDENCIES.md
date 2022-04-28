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

3. Openssh server: `sudo apt install openssh-server && service ssh start`

### Alpine Linux

1. GitHub CLI: see https://github.com/cli/cli/blob/trunk/docs/install_linux.md#alpine-linux

### Arch Linux

1. Sudo: `echo "y" | pacman -S sudo`

2. GitHub CLI: see https://github.com/cli/cli/blob/trunk/docs/install_linux.md#arch-linux

3. SSH group: `sudo groupadd -f ssh`

### CentOS

1. Sudo

```shell
dnf --disablerepo '*' --enablerepo=extras swap -y centos-linux-repos centos-stream-repos
dnf -y distro-sync
dnf install -y sudo
```

2. GitHub CLI: see https://github.com/cli/cli/blob/trunk/docs/install_linux.md#fedora-centos-red-hat-enterprise-linux-dnf

3. SSH group: `groupadd ssh`
