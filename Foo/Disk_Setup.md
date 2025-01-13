# Disk Setup

# This will be converted to a playbook once I have a better idea of all the steps/tasks that are needed
ansible -i inventories/kubernerdes.lab/hosts all -m shell -a "sudo df -h"

ansible-playbook -i inventories/kubernerdes.lab/hosts modules/00_install_lvm2.yml
ansible-playbook -i inventories/kubernerdes.lab/hosts modules/01_manage_disk.yml
ansible-playbook -i inventories/kubernerdes.lab/hosts modules/02_create_vols_fs.yml


## Cleanup
ansible -i inventories/kubernerdes.lab/hosts all -m shell -a "sudo lvremove lv_opt vg_nvme"

ansible -i inventories/kubernerdes.lab/hosts all -m shell -a "sudo lvremove /dev/vg_nvme/lv_opt"


lvremove /dev/vg_nvme/lv_docker
lvremove /dev/vg_nvme/lv_opt
lvremove /dev/vg_nvme/lv_usr_local
vgremove vg_nvme
pvremove /dev/nvme0n1p1
pvremove /dev/nvme0n1
