# Example Commands

## Run ad-hoc command on all hosts 
```bash
ansible -i inventories/kubernerdes.lab/hosts all -m shell -a "nvpmodel -q"
```

## Run a specific playbook on all hosts
```bash
ansible-playbook -i inventories/kubernerdes.lab/hosts playbooks/disable_ipv6.yml
```

## Run a specific playbook on worker hosts
```bash
ansible -i inventories/kubernerdes.lab/hosts worker_nodes -m shell -a "nvpmodel -q"
```
