# 1. Create inventory

- `mkdir projekti`
- `cd projekti`
- `touch inventory.yml`
```
all:
  hosts:
    192.168.2.130
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