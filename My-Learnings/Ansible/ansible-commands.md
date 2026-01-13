
- [syntax-check](#syntax-check)
- [Dry-run](#dry-run)




# Syntax-check

```
ansible-playbook -i /home/thor/ansible/inventory-t2q6 \
  /home/thor/ansible/playbook-t2q6.yml \
  --syntax-check
```

# Dry-run

```
ansible-playbook -i /home/thor/ansible/inventory-t2q6 \
  /home/thor/ansible/playbook-t2q6.yml \
  --check
```
