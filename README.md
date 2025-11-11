# Create-Ceph-Storage-Cluster-On-VM's
## Create Two Machines on vm 
```bash
| Hostname   | IP            | Role                      |
| ---------- | ------------- | ------------------------- |
| ceph-admin | <vm machine ip> | bootstrap + monitor + OSD |
| ceph-node1 | <vm machine ip> | OSD node                  |
```
## Put hostname and ip address value in /etc/hosts file (for example )
```bash
192.168.56.10 ceph-admin
192.168.56.11 ceph-node1
```
## Software Setup on both vm's
```bash
sudo dnf install -y chrony podman  python3 lvm2 crun
```
## Install cephadm on the machine:
```bash
sudo tee /etc/yum.repos.d/ceph.repo << 'EOF'
[ceph]
name=Ceph 18 (Reef) on RHEL9
baseurl=https://download.ceph.com/rpm-18.2.2/el9/x86_64/
enabled=1
gpgcheck=1
gpgkey=https://download.ceph.com/keys/release.asc
EOF
```
```bash
sudo rpm --import https://download.ceph.com/keys/release.asc
```
```bash
sudo dnf install -y cephadm
```
```bash
cephadm version
```
## Upgrade the packages 
```bash
dnf upgrade -y crun podman
```
## Passwordless SSH:
```bash
ssh-keygen
ssh-copy-id root@ceph-node1
```
## 🚀 Ceph Bootstrap (on ceph-admin)
```bash
cephadm bootstrap --mon-ip 192.168.56.10 --single-host-defaults
```
## Gomto ceph shell
```bash
cephadm shell
```
## Add node1 in cluster 
```bash
ceph orch host add ceph-node1 <ip of node1 vm>
```
## Add Storage Devices check storage device
```bash
ceph orch device ls
```
## Add OSDs:
```bash
ceph orch daemon add osd ceph-admin:/dev/sdb
ceph orch daemon add osd ceph-node1:/dev/sdb
```
## Verify Cluster
```bash
ceph -s
```
## Get dashboard URL:
```bash
ceph mgr services
```
