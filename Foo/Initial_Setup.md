# pre-req initial steps

## Purpose
I created this as there are steps needed to get the nodes with newly installed OS ready for Ansible updates.  

* Configure SSH keys for nvidia user to all the hosts
* SSH to host and add Ansible User with home directory not in /home (there is a reason)
* Create sudo for Ansible User
* Configure ssh config to default to Ansible User 

## Todo
I need to figure out how to get these initial tasks knocked out in a more scalable way.  But... this works (for now) and I'd rather invest my time in the "difficult tasks"

## Commands and Tasks
Set an environment variable for the hosts you wish to manage (will be used throughout this page)
```
HOSTS="1 2 3"
```
### Cleanup existing keys (because I rebuild my environemnt quite frequently)
```bash
for HOST in ${HOSTS}; do ssh-keygen -f "/home/mansible/.ssh/known_hosts.kubernerdes.lab" -R "10.10.12.21${HOST}"; done
for HOST in ${HOSTS}; do ssh-keygen -f "/home/mansible/.ssh/known_hosts.kubernerdes.lab" -R "xavier-0${HOST}.kubernerdes.lab"; done
for HOST in ${HOSTS}; do echo "ssh-keygen -f \"/home/mansible/.ssh/known_hosts.kubernerdes.lab\" -R \"xavier-0${HOST}\" "; done
for HOST in ${HOSTS}; do ssh-keygen -f "/home/mansible/.ssh/known_hosts.kubernerdes.lab" -R "xavier-0${HOST}"; done
```

### Copy SSH key from management node to Jetson nodes
TODO: Update this to automatically accept ssh fingerprint
```bash
echo "# BEGIN -- SSH Key Copy"
echo "# Note: This is about to prompt you for your nvidia user's password."
for HOST in ${HOSTS}; do ssh-copy-id -o StrictHostKeyChecking=accept-new nvidia@xavier-0${HOST}.kubernerdes.lab; done
echo "# END -- SSH Key Copy"
```

### Add User for Ansible (mansible "My Ansible")
```bash
echo "# Note: This is about to prompt you for your nvidia user's password."
for HOST in $HOSTS
do
  ssh -t nvidia@xavier-0$HOST.kubernerdes.lab "
  sudo mkdir -p /etc/sudoers.d
  sudo useradd -m -Gadm,sudo -u1001 -c 'My Ansible' -d /var/mansible -s /bin/bash -p '\$y\$j9T\$Ug0Hazie0m6D4TXqkk0Uh0\$fEB2zDsPbm6FIl3tvT9KoXXQBUVkOj3LhP4/pjjbty9' mansible
  echo 'mansible ALL=(ALL) NOPASSWD: ALL' | sudo tee /etc/sudoers.d/mansible-nopasswd-all"
done
```

### Copy SSH Key to hosts, then test SSH + sudo connection
```bash
echo "# Note: This is about to prompt you for your mansible user's password."
for HOST in ${HOSTS}; do ssh-copy-id mansible@xavier-0${HOST}.kubernerdes.lab; done
for HOST in ${HOSTS}; do ssh -o StrictHostKeyChecking=accept-new -t mansible@xavier-0${HOST}.kubernerdes.lab "sudo uptime"; done
for HOST in ${HOSTS}; do ssh -o StrictHostKeyChecking=accept-new -t mansible@xavier-0${HOST} "sudo uptime"; done
for HOST in ${HOSTS}; do ssh -o StrictHostKeyChecking=accept-new -t mansible@10.10.12.21${HOST} "sudo uptime"; done
```

### Test with Ansible ad-hoc command
Prereqs and assumptions:
* you need to be in the Ansible directory (as you will see we use relative paths here)
```bash
ansible-inventory -i inventories/kubernerdes.lab/hosts --list -y
ansible -i inventories/kubernerdes.lab/hosts all -m ping
```
