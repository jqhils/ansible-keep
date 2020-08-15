# 🏰 Ansible KEEP Playbook Collection
> Trust Math, Not Hardware.
Sets of ansible playbooks for deploying, monitoring and managing KEEP validator instances.
Currently supports Ropsten. Main net in the works.


# PREREQUISITES
* Ansible
    * [Install Guide](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
* Ubuntu 20.04 LTS Server setup with a sudo-enabled user to operate ansible tasks. (recommend Linode)
    * Support for other OS families scoped.


# Configuration
## Step 1
Change the ./vars/vars.yaml varaiables configuration file accordingly.

## Step 2
Create an inventory file & parse or edit the default /etc/ansible/hosts file. E.g.
```
[keep]
166.166.166.69

[keep:vars]
ansible_ssh_user = ansible
ansible_ssh_private_key_file = /Users/jqhils/.ssh/privatekey
```

# Usage

Execute the bootstrap playbook providing the required components and entering user sudo pw:
```bash
ansible-playbook bootstrap_keep.yaml -i hosts -K
```

After successfully setting up keep instances, explore the other additional playbooks:

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
- [ ] Main Net