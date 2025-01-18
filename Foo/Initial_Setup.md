# pre-req initial steps

## Purpose
I created this as steps needed to get the nodes with newly installed OS ready for Ansible updates

* Configure SSH keys for nvidia user to all the hosts
* SSH to host and add Ansible User with home directory not in /home (there is a reason)
* Create sudo for Ansible User
* Configure ssh config to default to Ansible User 

## Todo
I need to figure out how to get these initial tasks knocked out in a more scalable way.  But... this works (for now) and I'd rather invest my time in the "difficult tasks"

## Commands and Tasks
### Cleanup existing keys (because I rebuild my environemnt quite frequently)
```bash
for HOST in 1 2 3
do 
  ssh-keygen -f "/home/mansible/.ssh/known_hosts.kubernerdes.lab" -R "10.10.12.21${HOST}"
  ssh-keygen -f "/home/mansible/.ssh/known_hosts.kubernerdes.lab" -R "xavier-0${HOST}.kubernerdes.lab"
done
```

### Copy SSH key from management node to Jetson nodes
TODO: Update this to automatically accept ssh fingerprint
```bash
echo "NOTE: Cut-and-paste the text between BEGIN/END and enter yes and the password for nvidia user"
echo "# BEGIN"
for HOST in 1 2 3; do echo "ssh-copy-id nvidia@xavier-0${HOST}.kubernerdes.lab"; done
echo "# END"
```

### Add User for Ansible (mansible "My Ansible")
```bash
HOSTS=" 1 2 3"
for HOST in $HOSTS
do
  ssh -t nvidia@xavier-0$HOST.kubernerdes.lab "
  sudo mkdir -p /etc/sudoers.d
  sudo useradd -m -Gadm,sudo -u1001 -c 'My Ansible' -d /var/mansible -s /bin/bash -p '\$y\$j9T\$Ug0Hazie0m6D4TXqkk0Uh0\$fEB2zDsPbm6FIl3tvT9KoXXQBUVkOj3LhP4/pjjbty9' mansible
  echo 'mansible ALL=(ALL) NOPASSWD: ALL' | sudo tee /etc/sudoers.d/mansible-nopasswd-all"
done
```

### Test SSH + sudo connection
```bash
for HOST in 1 2 3; do ssh-copy-id mansible@xavier-0${HOST}.kubernerdes.lab; done
for HOST in 1 2 3; do ssh -t mansible@xavier-0${HOST}.kubernerdes.lab "sudo uptime"; done
```

### Test with Ansible ad-hoc command
Prereqs and assumptions:
* you need to be in the Ansible directory (as you will see we use relative paths here)
```bash
ansible-inventory -i inventories/kubernerdes.lab/hosts --list -y
ansible -i inventories/kubernerdes.lab/hosts all -m ping


```
