sudo docker run -it --rm --runtime nvidia --network host nvcr.io/nvidia/l4t-pytorch:r32.6.1-pth1.9-py3 --security-opt seccomp=unconfined

wget https://launchpad.net/ubuntu/+source/docker.io/20.10.2-0ubuntu1~18.04.2/+build/21335731/+files/docker.io_20.10.2-0ubuntu1~18.04.2_arm64.deb
sudo dpkg -i docker.io_20.10.2-0ubuntu1~18.04.2_arm64.deb
rm docker.io_20.10.2-0ubuntu1~18.04.2_arm64.deb
sudo apt install containerd=1.5.2-0ubuntu1~18.04.3


cat << EOF > /etc/apt/preferences
Package: docker.io
Pin: version 20.10.2*
Pin-Priority: 1001

Package: containerd
Pin: version 1.5.2*
Pin-Priority: 1001
EOF

curl -sfL https://get.k3s.io | sh -s - --docker
scp /etc/rancher/k3s/k3s.yaml mansible@10.10.12.10:.kube/k3s.kubeconfig
K3S_TOKEN=$(cat /var/lib/rancher/k3s/server/node-token)
curl -sfL https://get.k3s.io | K3S_URL=https://10.10.12.211:6443 K3S_TOKEN=$MY_K3S_TOKEN sh -s - --docker

# This caused it to not boot successfully
update /boot/extlinux/extlinux.conf -
add ipv6.disable=1
 to APPEND ${cbootargs} root=/dev/mmcblk0p1 rw rootwait rootfstype=ext4 console=ttyTCU0,115200n8 console=tty0 fbcon=map:0 net.ifnames=0 video=efifb:off nospectre_bhb nv-auto-config


# Run pod to ensure nvidia runtime works
docker run --rm --runtime nvidia -it nvcr.io/nvidia/l4t-base:r32.4.3

# Possible workaround for pkg install

sudo apt install libnvidia-container-tools libnvidia-container0:arm64 nvidia-container-csv-cuda nvidia-container-csv-tensorrt nvidia-container-runtime nvidia-container-toolkit nvidia-docker2

