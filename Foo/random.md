```
sudo docker run -it --rm --runtime nvidia --network host nvcr.io/nvidia/l4t-pytorch:r32.6.1-pth1.9-py3 --security-opt seccomp=unconfined

wget https://launchpad.net/ubuntu/+source/docker.io/20.10.2-0ubuntu1~18.04.2/+build/21335731/+files/docker.io_20.10.2-0ubuntu1~18.04.2_arm64.deb
sudo dpkg -i docker.io_20.10.2-0ubuntu1~18.04.2_arm64.deb
rm docker.io_20.10.2-0ubuntu1~18.04.2_arm64.deb
sudo apt install containerd=1.5.2-0ubuntu1~18.04.3
```

```
cat << EOF > /etc/apt/preferences
Package: docker.io
Pin: version 20.10.2*
Pin-Priority: 1001

Package: containerd
Pin: version 1.5.2*
Pin-Priority: 1001
EOF
```

```
curl -sfL https://get.k3s.io | sh -s - --docker
scp /etc/rancher/k3s/k3s.yaml mansible@10.10.12.10:.kube/k3s.kubeconfig
K3S_TOKEN=$(cat /var/lib/rancher/k3s/server/node-token)
curl -sfL https://get.k3s.io | K3S_URL=https://10.10.12.211:6443 K3S_TOKEN=$MY_K3S_TOKEN sh -s - --docker
```

# This caused it to not boot successfully
```
update /boot/extlinux/extlinux.conf -
add ipv6.disable=1
 to APPEND ${cbootargs} root=/dev/mmcblk0p1 rw rootwait rootfstype=ext4 console=ttyTCU0,115200n8 console=tty0 fbcon=map:0 net.ifnames=0 video=efifb:off nospectre_bhb nv-auto-config
```


# Run pod to ensure nvidia runtime works
```
docker run --rm --runtime nvidia -it nvcr.io/nvidia/l4t-base:r32.4.3
```

# Possible workaround for pkg install

```
sudo apt install libnvidia-container-tools libnvidia-container0:arm64 nvidia-container-csv-cuda nvidia-container-csv-tensorrt nvidia-container-runtime nvidia-container-toolkit nvidia-docker2

# curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC='--flannel-backend=none --disable-network-policy --docker' sh -

```
# Trying cilium
## On K3s node
```
export INSTALL_K3S_VERSION="v1.29.12+k3s1"
export INSTALL_K3S_EXEC='--flannel-backend=none --disable-network-policy --docker --advertise-address=10.10.12.210'
export INSTALL_K3S_EXEC='--docker --advertise-address=10.42.0.1 '
export INSTALL_K3S_EXEC='--docker --prefer-bundled-bin '
curl -sfL https://get.k3s.io |  sh -
scp /etc/rancher/k3s/k3s.yaml mansible@10.10.12.10:.kube/k3s.kubeconfig
```

```
## Run on kubernerd
# Get PodCIDR for this node
export KUBECONFIG=~/.kube/k3s.kubeconfig
sed -i -e 's/127.0.0.1/10.10.12.211/g' $KUBECONFIG
PODCIDR=$(kubectl get nodes -o jsonpath='{.items[*].spec.podCIDR}')
echo "cilium install --version 1.16.6 --set=ipam.operator.clusterPoolIPv4PodCIDRList=\"${PODCIDR}\""
cilium install --version 1.16.6 --set=ipam.operator.clusterPoolIPv4PodCIDRList="${PODCIDR}"
```

```
MODULES="ipt_REJECT
xt_mark
xt_multiport
xt_TPROXY
xt_CT
sch_ingress
ip_set
cls_bpf"
for MOD in $MODULES; do lsmod | grep $MOD || { insmod $MOD; } && { echo "$MOD found"; };  done
```

## Latest install method (x86 on VMWare guests)
https://documentation.suse.com/trd/suse/html/kubernetes_ri_rancher-k3s-slemicro/id-deployment.html
### Install with embedded-etcd
First System
```
curl -sfL https://get.k3s.io | \
	INSTALL_K3S_VERSION=${K3s_VERSION} \
	INSTALL_K3S_SKIP_SELINUX_RPM=true \
	INSTALL_K3S_EXEC='server --cluster-init --write-kubeconfig-mode=644' \
	sh -s -
```

Second/Third System
```
# Private IP preferred, if available
export FIRST_SERVER_IP="10.10.12.181"
# From /var/lib/rancher/k3s/server/node-token file on the first server
export NODE_TOKEN="K10d055e962fafd5871ed004000696035aa9fa8137f1c6e423d329804abc34363f6::server:bf62ae76e05e55d04d29c9624e3cdc1c"
# Match the first of the first server
export K3s_VERSION="v1.31"
curl -sfL https://get.k3s.io | \
	INSTALL_K3S_VERSION=${K3s_VERSION} \
	INSTALL_K3S_SKIP_SELINUX_RPM=true \
	K3S_URL=https://${FIRST_SERVER_IP}:6443 \
	K3S_TOKEN=${NODE_TOKEN} \
	K3S_KUBECONFIG_MODE="644" INSTALL_K3S_EXEC='server' \
	sh -
```

Agent Systems
```
curl -sfL https://get.k3s.io | \
	INSTALL_K3S_VERSION=${K3s_VERSION} \
	INSTALL_K3S_SKIP_SELINUX_RPM=true \
	K3S_URL=https://${FIRST_SERVER_IP}:6443 \
	K3S_TOKEN=${NODE_TOKEN} \
	K3S_KUBECONFIG_MODE="644" \
	sh -
```
