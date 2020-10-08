# Vagrant with ansible_local

To [Vagrant with ansible](https://github.com/rurumimic/vagrant-ansible)

---

Vagrant: [Ansible Local Provisioner](https://www.vagrantup.com/docs/provisioning/ansible_local)

## Downloads

- [Vagrant](https://www.vagrantup.com/downloads)
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)

---

## Files

### Current directory

- [.gitignore](.gitignore): Ignore [Vagrant](https://www.toptal.com/developers/gitignore/api/vagrant,macos,code) and Certficates
- [Vagrantfile](Vagrantfile): Set VMs (instances, cpu, memory, network, security...)
- **provision**: Ansible configurations
  - [ansible.cfg](provision/ansible.cfg)
    - Doc: [Configuring Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_configuration.html)
  - [inventory](provision/inventory)
    - Doc: [How to build your inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html)
  - [playbook.yml](provision/playbook.yml)
    - Doc: [Working with playbooks](https://docs.ansible.com/ansible/latest/user_guide/playbooks.html)
- _ca-trust (option)_: _for enterprise certificates (self-signed)_
  - _enterprise.crt.sample_

### External

- `~/.vagrant.d/insecure_private_key`: [Insecure keypair](https://github.com/hashicorp/vagrant/tree/master/keys) for SSH & Ansible inventory

### [Synced Folders](https://www.vagrantup.com/docs/synced-folders)

By default, Vagrant will share your project directory (the directory with the `Vagrantfile`) to `/vagrant`.

---

## Set up

### (Option) Add a enterprise CA as a trusted certificate authority

```bash
cp /path/to/enterprise.crt ./ca-trust
```

## Start

### Provision

```bash
vagrant up
```

**Result**:

```bash
PLAY [nodes]

TASK [Gathering Facts]
ok: [node1]
ok: [node2]

TASK [Who am I?]
TASK [Ensure ntpd is at the latest version]
RUNNING HANDLER [Restart ntpd]

PLAY RECAP
node1: ok=4    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
node2: ok=4    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

### Retry Ansible playbook

### Host machine

```bash
vagrant reload controller --provision
```

### Guest machine

```bash
vagrant ssh controller # SSH into a running Vagrant machine

cd /vagrant

PYTHONUNBUFFERED=1 \
ANSIBLE_FORCE_COLOR=true \
ANSIBLE_CONFIG='provision/ansible.cfg' \
ansible-playbook \
--limit="all" \
--inventory-file=provision/inventory \
-v provision/playbook.yml
```

### Other commands

```bash
vagrant status
vagrant up
vagrant ssh node1
vagrant ssh node2
vagrant ssh controller
vagrant halt
vagrant destroy [-f]
```
