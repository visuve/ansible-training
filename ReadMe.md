# 1. Create inventory

- `mkdir projekti`
- `cd projekti`
- `touch inventory.yml`
```
all:
  hosts:
    192.168.2.130
  vars:
    ansible_user: ite
```

# 2. Test inventory
- `ansible all -m ping`

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
  - `ssh-copy-id -i ~/avain ite@192.168.2.130`
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

- `roles/glances/handlers/main.yml`
- `roles/glances/handlersr/main.yml`
- `roles/glances/templates/glancesweb.service.j2`

# 6. Implement playbook
- `touch playbook.yml`
