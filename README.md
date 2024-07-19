# Create Multi Master pgEdge Setup

## Requirements

- Two Linux servers with Oracle Linux version 9 installed.
- A user on both servers with sudo rights without password:
``` bash
echo "<your_user> ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/<your_user> >/dev/null
```

### Ansible Requirements

Ensure the following are installed on the Ansible executing machine:

``` bash
ansible --version
```
ansible [core 2.14.14] <br />
  python version = 3.10.12 <br />
  jinja version = 3.0.3 <br />
  libyaml = True <br />
  <br />

``` bash
ansible-galaxy collection list
```
Collection           Version <br />
ansible.posix        1.5.4 <br />
community.general    9.1.0 <br />
community.postgresql 3.4.1 <br />

## Role Variables
``` yaml
[db_primary]
pgedge1

[db_standby]
pgedge2

[db:children]
db_primary
db_standby

[db:vars]
n1_ip=192.168.56.112
n1_name=pgedge1
n2_ip=192.168.56.111
n2_name=pgedge2
pgcluster=rg-prod
pgedge_user=admin
non_root_user=sinwi
port=5432
``` 

## Valid values
``` yaml
```

### Creating password file for db_user:
----------------
``` bash
ansible-vault encrypt_string 'your_password_here' --name 'db_superuser_password' > inventories/prod/group_vars/all/vault.yml
```

### Example command-line to create the initial install
----------------
``` bash
ansible-playbook -i inventories/prod/ postgres-pri-stb.yml --ask-vault-pass
```

## List of databases, roles and installed extensions
``` sql
```

License
----------------
itm8 a/s

Author Information
----------------
"Simon Nimb Williams" <sinwi@itm8.com>