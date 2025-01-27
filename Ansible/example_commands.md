# Example Commands

## Run a bunch of playbooks ad-hoc
PLAYBOOKS="enable_sudo_nopasswd.yml
configure_nvidia_power_mode.yml
disable_ipv6.yml
update_etc_hosts.yml"
for PLAYBOOK in $PLAYBOOKS
do
  ansible-playbook -l xavier-01.kubernerdes.lab -i inventories/kubernerdes.lab/hosts playbooks/$PLAYBOOK
done

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
ansible -i inventories/kubernerdes.lab/hosts worker_nodes -m shell -a "nvpmodel -q"
ansible-playbook -l xavier-01.kubernerdes.lab -i inventories/kubernerdes.lab/hosts playbooks/configure_nvidia_power_mode.yml
```

## Run deviceQuery and test nvidia-container-runtime
```bash
ansible -i inventories/kubernerdes.lab/hosts all -m shell -a "docker run --rm --runtime nvidia xift/jetson_devicequery:r32.5.0"
ansible -i inventories/kubernerdes.lab/hosts all -m shell -a "nvidia-container-runtime --version"
```

## Random Commands
```bash
ansible -i inventories/kubernerdes.lab/hosts all -m shell -a "docker run --rm --runtime nvidia nvidia/cuda:12.0.0-base-ubuntu22.04 nvidia-smi"
```


## Note
nvidia-smi does not/will not run on the NVIDIA Jetson devices.
```
ansible -i inventories/kubernerdes.lab/hosts all -m shell -a "nvidia-smi"
sudo docker run --rm --gpus all nvidia/cuda:11.6.2-base-ubuntu20.04 nvidia-smi
docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: nvidia-container-runtime did not terminate successfully: exit status 1: unknown.non-zero return code
```
