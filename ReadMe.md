# 0. Intro

- This is an introduction how to automate software installation & configuration using Ansible
- Basically automates this gist: https://gist.github.com/visuve/ddebd92efda4eda0ee89cbc5d9a7ecf8

# 1. Create inventory

- `mkdir projekti`
- `cd projekti`
- `touch inventory.yml`
```
all:
  hosts:
    10.0.x.xxx
  vars:
    ansible_user: ansible
```

# 2. Test inventory
- `ansible all -i inventory.yml -m ping`

- Should fail with:
```
192.168.2.130 | UNREACHABLE! => {
    "changed": false, 
    "msg": "Failed to connect to the host via ssh: Permission denied (publickey,password).\r\n", 
    "unreachable": true
}
```

# 3. Generate SSH-keys

- On the seed machine
  - `ssh-keygen -f ~/avain`
  - `ssh-copy-id -i ~/avain ansible@10.0.x.xxx`
  - `ssh-add ~/avain`
  
# 4. Test inventory
- `ansible all -i inventory.yml -m ping`

- Should succeed with:
```
192.168.2.130 | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
```

# 4. Generate role for Glances

- Create directories
- `mkdir -p roles/glances/tasks`
- `mkdir -p roles/glances/handlers`
- `mkdir -p roles/glances/templates`

- Create skeletons
- `touch roles/glances/tasks/main.yml`
- `touch roles/glances/handlers/main.yml`
- `touch roles/glances/templates/glancesweb.service.j2`

# 5. Fill skeletons

- `nano roles/glances/tasks/main.yml`
- `nano roles/glances/handlers/main.yml`
- `nano roles/glances/templates/glancesweb.service.j2`

# 6. Implement playbook
- `touch playbook.yml`

# 7. Run playbook

- `ansible-playbook -i inventory.yml playbook.yml`
- Should fail with:
```
...
open (13: Permission denied)\nE: Unable to lock the administration directory (/var/lib/dpkg/), are you root?\n", "stdout": "", "stdout_lines": []}
...
```

# 8. Add "become" in tasks where sudo is needed

E.g.
```
- name: Install Glances
  become: true
  . . .
```

- Should still fail with:
```
...
"module_stdout": "sudo: a password is required\r\n"
```

# 9. Either hack things or do it right...

- Add `ansible_become_pass` in inventory.yml
- `ansible-vault encrypt inventory.yml`

- Note: if you can't stand vi:
  - `nano ~/.bashrc`
  - `export EDITOR=nano`
  - `. ~/.bashrc`
  
# 10. Finally run our playbook!
- `ansible-playbook --ask-vault-pass -i inventory.yml playbook.yml`

# 11. BONUS: automate the UFW as well ;-)
