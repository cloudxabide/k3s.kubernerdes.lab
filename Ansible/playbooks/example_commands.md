# Example Commands

## Run ad-hoc command on all hosts 
```bash
ansible -i inventories/kubernerdes.lab/hosts all -m shell -a "nvpmodel -q"
```

## Run a specific playbook on all hosts
```bash
ansible-playbook -i inventories/kubernerdes.lab/hosts playbooks/disable_ipv6.yml
```
## Test Docker
```bash
ansible -i inventories/kubernerdes.lab/hosts all -m shell -a "docker run --rm --runtime nvidia hello-world"

## Run a specific command or playbook on specific nodes or group
```bash
ansible -i inventories/kubernerdes.lab/hosts all -m shell -a "nvidia-smi"
ansible -i inventories/kubernerdes.lab/hosts worker_nodes -m shell -a "nvpmodel -q"
ansible-playbook -l xavier-01.kubernerdes.lab -i inventories/kubernerdes.lab/hosts playbooks/configure_nvidia_power_mode.yml
```

## Run deviceQuery
```bash
ansible -i inventories/kubernerdes.lab/hosts all -m shell -a "docker run --rm --runtime nvidia xift/jetson_devicequery:r32.5.0"
ansible -i inventories/kubernerdes.lab/hosts all -m shell -a "sudo docker run --rm --runtime=nvidia --gpus all ubuntu nvidia-smi"

ansible -i inventories/kubernerdes.lab/hosts all -m shell -a "docker run --rm --gpus all nvidia/cuda:12.0.0-base-ubuntu22.04 nvidia-smi"

```

## Expiremental
```bash
sudo docker run --rm --runtime=nvidia
```
