# Example Commands

## Run a bunch of playbooks ad-hoc
```bash
PLAYBOOKS="enable_sudo_nopasswd.yml
configure_nvidia_power_mode.yml
disable_ipv6.yml
disable_swap.yml
update_etc_hosts.yml
storage_manage_disk.yml
storage_vols_fs.yml
storage_home_directory.yml
storage_opt_directory.yml
storage_usr_local_directory.yml
"

OTHER_PLAYBOOKS="
install_kubernetes_utils.yml
install_nvidia_ctk_and_docker.yml
update_docker_runtime_for_nvidia.yml
install_nvidia_prereq_pkgs.yml
install_pytorch.yml
install_K3s.yml
update_os.yml"

for PLAYBOOK in $PLAYBOOKS
do
  ansible-playbook -l xavier-01.kubernerdes.lab -i inventories/kubernerdes.lab/hosts playbooks/$PLAYBOOK
done
```

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
nvidia-smi does not/will not run on the NVIDIA Jetson devices due to way the hardware is implemented.
```
ansible -i inventories/kubernerdes.lab/hosts all -m shell -a "nvidia-smi"
sudo docker run --rm --gpus all nvidia/cuda:11.6.2-base-ubuntu20.04 nvidia-smi
docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: nvidia-container-runtime did not terminate successfully: exit status 1: unknown.non-zero return code
```
## List of playbooks
<pre>
configure_nvidia_power_mode.yml
disable_ipv6.yml
disable_swap.yml
disable_ufw.yml
enable_sudo_nopasswd.yml
install_K3s.yml
install_kubernetes_utils.yml
install_nvidia_ctk_and_docker.yml
install_nvidia_prereq_pkgs.yml
install_pytorch.yml
storage_home_directory.yml
storage_manage_disk.yml
storage_opt_directory.yml
storage_usr_local_directory.yml
storage_vols_fs.yml
update_docker_runtime_for_nvidia.yml
update_etc_hosts.yml
update_os.yml

</pre>
