# ovirt
oVirt test installation on Rocky Linux

## From oVirt Site
Installing on RHEL 9.0 or derivatives
Specific to RHEL only:

subscription-manager repos --enable rhel-9-for-x86_64-baseos-rpms
subscription-manager repos --enable rhel-9-for-x86_64-appstream-rpms
subscription-manager repos --enable codeready-builder-for-rhel-9-x86_64-rpms
subscription-manager repos --enable rhel-9-for-x86_64-resilientstorage-rpms

rpm -i --justdb --nodeps --force "http://mirror.stream.centos.org/9-stream/BaseOS/$(rpm --eval '%_arch')/os/Packages/centos-stream-release-9.0-26.el9.noarch.rpm"
NOTE If installation fails on centos-stream-release-9.0-26.el9.noarch.rpm please check the content of the repository for a newer version of that package.

Common to RHEL 9.0 and derivatives:

cat >/etc/yum.repos.d/CentOS-Stream-Extras-common.repo <<'EOF'
[c9s-extras-common]
name=CentOS Stream $releasever - Extras packages
metalink=https://mirrors.centos.org/metalink?repo=centos-extras-sig-extras-common-$stream&arch=$basearch&protocol=https,http
gpgkey=https://www.centos.org/keys/RPM-GPG-KEY-CentOS-SIG-Extras
gpgcheck=1
repo_gpgcheck=0
metadata_expire=6h
countme=1
enabled=1
EOF

echo "9-stream" > /etc/yum/vars/stream

dnf distro-sync --nobest
Due to Bug 2091581, while installing the virtualization host you will be missing nmstate-plugin-ovsdb or python3-libnmstate unless you install them from CentOS build nmstate-2.0.0-2.el9. While installing it you need to ensure openvswitch2.15 will be installed instead of any later version.

Here’s how to do that:

dnf install -y centos-release-ovirt45
dnf install -y ovirt-openvswitch
dnf install -y \
https://kojihub.stream.centos.org/kojifiles/packages/nmstate/2.0.0/2.el9/noarch/nmstate-plugin-ovsdb-2.0.0-2.el9.noarch.rpm \
https://kojihub.stream.centos.org/kojifiles/packages/nmstate/2.0.0/2.el9/noarch/python3-libnmstate-2.0.0-2.el9.noarch.rpm

## CMDs from ChatGPT
sudo dnf update -y
sudo dnf install -y epel-release

sudo hostnamectl set-hostname ovirt-node.example.com

sudo setenforce 0
sed -i 's/^SELINUX=.*/SELINUX=permissive/' /etc/selinux/config

sudo dnf install -y https://resources.ovirt.org/pub/yum-repo/ovirt-release45.rpm

sudo dnf install -y ovirt-engine

sudo engine-setup

https://<IP_del_tuo_server>/ovirt-engine


