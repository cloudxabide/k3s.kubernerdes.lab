# pre-req initial steps

## Purpose
I created this as steps needed to get the nodes with newly installed OS ready for Ansible updates


## Todo
I need to figure out how to get these initial tasks knocked out in a more scalable way.  But... this works (for now) and I'd rather invest my time in the "difficult tasks"

## Commands and Tasks
### Cleanup existing keys (because I rebuild my environemnt quite frequently)
```bash
for HOST in 1 2 3; do   ssh-keygen -f "/home/mansible/.ssh/known_hosts.kubernerdes.lab" -R "10.10.12.21${HOST}"; done
```

### Copy SSH key from management node to Jetson nodes
TODO: Update this to automatically accept ssh fingerprint
```bash
for HOST in 1 2 3; do echo "ssh-copy-id nvidia@xavier-0${HOST}.kubernerdes.lab"; done
```

### Update Sudo for NOPASSWD
```bash
HOSTS=" 1 2 3"
for HOST in $HOSTS
do
ssh-keygen -f "/home/mansible/.ssh/known_hosts.kubernerdes.lab" -R "xavier-0${HOST}.kubernerdes.lab"
ssh -t nvidia@xavier-0$HOST.kubernerdes.lab "
sudo mkdir -p /etc/sudoers.d
echo 'nvidia ALL=(ALL) NOPASSWD: ALL' | sudo tee /etc/sudoers.d/nvidia-nopasswd-all"
done

for HOST in 1 2 3; do ssh -t nvidia@xavier-0${HOST}.kubernerdes.lab "sudo uptime"; done
```

### Test with Ansible ad-hoc command
Prereqs and assumptions:
* you need to be in the Ansible directory (as you will see we use relative paths here)
```
ansible-inventory -i inventories/kubernerdes.lab/hosts --list -y
ansible -i inventories/kubernerdes.lab/hosts all -m ping


```
