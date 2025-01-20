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
