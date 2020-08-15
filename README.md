# 🏰 Ansible KEEP Playbook Collection
> Trust Math, Not Hardware.
## This repository contains sets of ansible playbooks for deploying, monitoring and managing KEEP validator instances.

*NOTE:* The codebase will likely undergo a restructuring as I add additional capabilities.


# PREREQUISITES
* Ansible
    * [Install Guide](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
* Ubuntu 20.04 LTS Server setup with a sudo-enabled user to operate ansible tasks. (recommend Linode)


# SETUP
## Step 1
First step is configuring the keep_vars.yaml variable file that will be parsed. This can also be a json file.
```yaml
# keep_vars.yaml

# Ethereum Wallet Details
account:
  address: "0xAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA"
  password: "P@ssW0rd"

# Version of the docker image(s) to run.
docker:
  client_version: latest
  ecdsa_version: v1.2.0-rc

```

## Step 2
Configuring your hosts file. Either editing the default /etc/ansible/hosts file or parsing your own. E.g.:
```
# hosts

[keep]
166.166.166.69

[keep:vars]
ansible_ssh_user = ansible
ansible_ssh_private_key_file = /Users/jqhils/.ssh/privatekey
```

## Step 3
Execute the bootstrap playbook providing the required components:
```bash
ansible-playbook bootstrap.yaml -i /home/jqhils/hosts --extra-vars "@/home/jqhils/keep_vars.yaml" --extra-vars "wallet=/home/jqhils/wallet.json" -K
```

## Additionally...
Once you've bootstrapped and you want to deploy a configuration change. You can re-execute the command with the following tag to skip preliminary setup tasks. This will become reduntant in future releases:
 ```bash
 --skip-tags "dockersetup"
```

## Future plans 🚀:
- [x] Provisioning of docker and other related dependencies.
- [x] Deploys and executes Keep-client and Keep-ecdsa-client docker instances
- [X] Compartmentalize sets of tasks and provide greater non-linear control.
- [ ] Better security and secrets handling (Ansible Vaults)
- [ ] Alert notifications for network/instance failures & Network Upgrades.
- [ ] Setting up Log Monitoring and Pipelining(?) (E.g. [ElasticStack](https://www.notion.so/Setting-up-Elastic-Stack-Dashboard-14f9edc94418468bb95af40417a0332a))
- [ ] Cross-OS compatibility for docker-setup role
- [ ] More...