# 🏰 Ansible KEEP Playbook Collection
> Trust Math, Not Hardware.

Set of ansible playbooks for deploying, monitoring and managing KEEP validator instances.


# Prerequisites
* Ansible
    * [Install Guide](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
* Ubuntu Server setup with a sudo-enabled user to operate ansible tasks.
* The appropriate firewall configuration settings.
<br />

# KEEP Configuration
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

After successfully setting up keep instances, explore the other additional playbooks for setting up monitoring.


<br /><br />

# Configuring Promethus/Grafana Monitoring
The playbooks configure in the same manner as in mutedtommy's guide found here:

https://medium.com/@hr12rtk/keep-random-beacon-node-monitoring-grafana-prometheus-and-loki-4a4b669b31ea

## Step 1
Install the ansible role dependencies from galaxy:
```bash
ansible-galaxy install community.general
```

## Step 2
Install and configure the monitoring software using the bootstrap_monitoring playbook
```bash
ansible-playbook bootstrap_monitoring.yaml -i hosts -K
```

## Step 3
SSH in and run the docker-compose command in the following two directories:
- /home/ansible/prometheus/
- /home/ansible/loki/
```bash
sudo docker-compose up -d
```

## Step 4
Reconfigure and re-run docker image(s) to configure docker to use loki log driver for monitoring.
```bash
ansible-playbook configure_keep.yaml -i hosts -K
```

<br /><br />

# Configuring ELK (Experimental)
## Step 1
Create your own Elastic Cloud deployment. Follow the steps [here](https://www.notion.so/Setting-up-Elastic-Stack-Dashboard-14f9edc94418468bb95af40417a0332a)

## Step 2
Change the ./vars/elk_vars.yaml variables configuration file accordingly.

## Step 3
Install the ansible role dependencies from galaxy:
```bash
ansible-galaxy install geerlingguy.java geerlingguy.logstash geerlingguy.filebeat
```

# Usage

Execute the ELK Playbook:
```bash
ansible-playbook bootstrap_elk.yaml -i hosts -K
```

<br /><br />

## Playbooks
yaml | Description
-----|------------
`bootstrap_keep.yaml` | Installs & configures  [Keep-ecdsa](https://github.com/keep-network/keep-ecdsa) and [Keep-core](https://github.com/keep-network/keep-core/blob/master/docs/run-random-beacon.adoc)
`configure_keep.yaml` | Adds additional loki & monitoring configuration when running docker instance(s)
`bootstrap_monitoring` | Installs and configures grafana and prometheus for monitoring.
`bootstrap_elk.yaml` | Installs filebeat+logstash and ingests KEEP logs to ELK

<br />

## Additional tasks you should consider:
- Configuring email alerts via Grafana
- Automating a backup procedure of managed keys for ECDSA.