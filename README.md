# 🏰 Ansible KEEP Playbook Collection
> Trust Math, Not Hardware.

Set of ansible playbooks for deploying, monitoring and managing KEEP validator instances.


# Prerequisites
* Ansible
    * [Install Guide](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
* Ubuntu Server setup with a sudo-enabled user to operate ansible tasks.
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
Once you've bootstrapped and you want to deploy a configuration change. You can run the configure_keep.yaml playbook:
 ```bash
ansible-playbook configure_keep.yaml -i hosts -K
```

## Playbooks
yaml | Description
-----|------------
`bootstrap_keep.yaml` | Installs & configures  [Keep-ecdsa](https://github.com/keep-network/keep-ecdsa) and [Keep-core](https://github.com/keep-network/keep-core/blob/master/docs/run-random-beacon.adoc)
`configure_keep.yaml` | Applies configuration changes and restarts Keep-ecdsa & Keep-core

## Future plans 🚀:
- [x] Provisioning of docker and other related dependencies.
- [x] Deploys and executes Keep-client and Keep-ecdsa-client docker instances
- [X] Compartmentalize sets of tasks and provide greater non-linear control.
- [ ] Better security and secrets handling (Ansible Vaults)
- [ ] Cross-OS compatibility for docker-setup role
- [ ] Deployment of filebeat configurations for Log Monitoring using [ElasticStack](https://www.notion.so/Setting-up-Elastic-Stack-Dashboard-14f9edc94418468bb95af40417a0332a))