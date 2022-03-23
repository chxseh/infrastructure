<div align="center">
<h1>Infrastructure<br>
<a href="https://github.com/chxseh/infrastructure/blob/main/LICENSE.md"><img alt="GitHub license" src="https://img.shields.io/github/license/chxseh/infrastructure"></a>
<a href="https://github.com/chxseh/infrastructure/stargazers"><img alt="GitHub stars" src="https://img.shields.io/github/stars/chxseh/infrastructure"></a>
<a href="https://github.com/chxseh/infrastructure/issues"><img alt="GitHub issues" src="https://img.shields.io/github/issues/chxseh/infrastructure"></a>
<a href="https://github.com/chxseh/infrastructure/network"><img alt="GitHub forks" src="https://img.shields.io/github/forks/chxseh/infrastructure"></a>
</h1></div>

An Ansible playbook that sets up multiple Ubuntu-based servers.

It assumes a fresh Ubuntu Server 20.04 install, access to a non-root user with sudo privileges and a public SSH key. This can be configured during the installation process.

The playbook is mostly being developed for personal use, so stuff is going to be constantly changing and breaking. Use at your own risk and don't expect any help in setting it up on your machine.

## Table of Contents  <!-- omit in toc -->
- [Using This Playbook](#using-this-playbook)
- [Editing Secrets](#editing-secrets)
- [Additional Notes](#additional-notes)
  - [Change SSH Port](#change-ssh-port)
  - [Mail-in-a-Box](#mail-in-a-box)
- [Sister Repositories](#sister-repositories)
  - [Scripts](#scripts)
  - [dotfiles](#dotfiles)
- [Special Thanks](#special-thanks)

## Using This Playbook
> Before running this playbook, ensure SSH ports have been [changed](#change-ssh-port) to what's expected in [hosts](https://github.com/chxseh/infrastructure/blob/main/hosts).

```bash
sudo apt install ansible -y && git clone https://github.com/chxseh/infrastructure.git && cd infrastructure

# Initial Setup
ansible-galaxy install -r requirements.yml && ansible-playbook run.yml -i ./hosts -K --ask-vault-pass

# Subsequent Runs
ansible-galaxy install -r requirements.yml && ansible-playbook run.yml -i ./hosts --ask-vault-pass

# Sync Web Server Files from backup_servers[0]
[...] -e "sync_wsf=true"

# Sync Docker Volumes from backup_servers[0]
[...] -e "sync_docker=true"

# Sync Home Folder / GH CLI Runner Acc from backup_servers[0]
[...] -e "sync_hf_and_cli_acc=true"
```

## Editing Secrets

```bash
ansible-vault edit secret.yml
```

## Additional Notes

### Change SSH Port

```bash
# Before using this playbook, log into the server(s) and change SSH port.
sudo vim /etc/ssh/sshd_config
sudo systemctl restart sshd
```

### Mail-in-a-Box

This playbook does **not** install Mail-in-a-Box as MIAB has a GUI installation process.

To install MIAB run the following on the server(s):
```bash
curl -s https://mailinabox.email/setup.sh | sudo bash && bash ~/After_MIAB_Upgrade.sh
```

## Sister Repositories

### [Scripts](https://github.com/ChxseH/Scripts)

### [dotfiles](https://github.com/ChxseH/dotfiles)

## Special Thanks

* Alex Kretzschmar and Chris Fisher from [Self Hosted Show](https://selfhosted.show/) for introducing me to the idea of Infrastracture as Code
* Jeff Geerling for [Ansible 101 series](https://youtu.be/goclfp6a2IQ?list=PL2_OBreMn7FqZkvMYt6ATmgC0KAGGJNAN) on YouTube.
* notthebee for [IaC Deep Dive series](https://youtu.be/Z7p9-m4cimg) on YouTube.
