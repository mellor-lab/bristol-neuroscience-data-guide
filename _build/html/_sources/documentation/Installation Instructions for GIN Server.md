(doc-install-server)=
# Installation Instructions for GIN Server
There are five servers dedicated to Bristol GIN. Server[0] contains a Bristol GIN web frontend. Server[1] is the main node of the Bristol GIN data storage system housing [Ceph](https://ceph.io/en/) installation. All five servers are part of the Ceph cluster encompassing roughly 5TB of raw storage space in total. All data stored on Bristol GIN is either backed up twice or once by the Ceph storage system. Therefore, the real storage capacity varies between roughly 1.7TB to 2.5TB.

(doc-install-server-ceph)=
## Install Ceph
https://ceph.io/en/
https://docs.ceph.com/en/octopus/
https://docs.ceph.com/en/latest/start/intro/

(doc-install-server-ceph-cephadm)=
### Install Cephadm
The Ceph cluster is installed, deployed and managed using [cephadm](https://docs.ceph.com/en/quincy/cephadm/index.html) command line tools. Hence, we need to install cephadm first before we can deploy the Ceph cluster. We start by loging into Server[1]. This server is going to be the main node of the Ceph cluster.

If your sudo user is not passwordless, you need to make it passwordless. Open the sudoers file by typing in the Linux terminal:
```
sudo yum update
sudo yum install nano
sudo nano /etc/sudoers
```
Inside the file comment the line
```
%wheel        ALL=(ALL)       ALL
```
and uncomment the line below:
```
%wheel        ALL=(ALL)       NOPASSWD: ALL
```
Now you should be able to call sudo commands without being required to type in your password.

The next is to install cephadm dependencies. We start with Python 3:
```
sudo yum install python3
sudo nano ~/.bashrc
```
Add ```alias python=/usr/bin/python3``` line to your bash.rc script, save it, and reload it:
```
source ~/.bashrc
```
The latter addition to .bashrc file makes Python 3 as your primary Python interpreter.

Other cephadm dependencies include systemd and Podman. We install those next:
```
sudo yum install systemd
sudo yum -y install podman
```

[Docker](https://docs.docker.com/engine/install/centos) is yet another cephadm dependency. Docker installation is, however, optional if you have already installed Podman. In any case, the installation commands are below:
```
sudo yum install -y yum-utils
sudo yum-config-manager \
  --add-repo \
  https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

Install the remaining cephadm dependencies: chrony, ntp, and lvm2.
```
sudo yum install chrony
sudo yum install ntp
sudo yum install lvm2
```

We can now proceed with the actual cephadm installation:
```
curl --silent --remote-name --location https://github.com/ceph/ceph/raw/quincy/src/cephadm/cephadm
chmod +x cephadm
sudo ./cephadm add-repo --release octopus
sudo rpm --import 'https://download.ceph.com/keys/release.asc'
sudo ./cephadm install
```
As you can see, we did not install the latest Ceph version. This is because Linux Centos 7 operating system does not work with Ceph versions newer than Octopus.

Finally, enable Ceph command line interface (CLI):
```
sudo cephadm install ceph-common
```

(doc-install-server-ceph-cluster-install)=
### Install Ceph Cluster
Once cephadm is installed, install the Ceph cluster. We start by boostraping a new cluster:
```
sudo cephadm bootstrap --mon-ip <main-backend-server1-ip> --allow-fqdn-hostname --allow-overwrite --ssh-user <your-username>
```

Next, we add other hosts to the Ceph cluster. In order to do that, login to all servers (Server[2], Server[3], Server[4], and Server[0]) except the main one (Server[1]) and perform all prerequisite actions needed to add these servers to the Ceph cluster: setting a passwordless sudo access and installing all the required dependencies. Following that, install Ceph CLI:
```
sudo yum install ceph-common
```

We also need to create a logical volume on partition ```dev/sdb1``` of the data disk ```dev/sdb``` on all of our servers, including Server[1]. We start by preventing the automatic mounting of the ```/dev/sdb1``` partition onto ```/data``` folder. To do that, open ```/etc/fstab``` file:
```
sudo nano /etc/fstab
```
Inside the file replace the last line
```
UUID=<some-UUID>     /data       ext4   defaults
```
with the line
```
UUID=<some-UUID>     /data       ext4   defaults,noauto,user
```
Reboot the server. Finally, we create a volume group containing this partition which can be used to create a logical volume:
```
sudo vgcreate vg1 /dev/sdb1
sudo lvcreate -l 100%FREE -n lv1 vg1
```
Reboot each server again following the setup.

Now we can add the remaining hosts to the cluster. Login to the main Ceph node (Server[1]) and setup Ceph SSH access to all additional hosts:
```
ssh-copy-id -f -i /etc/ceph/ceph.pub <your-username>@<backend-server2-ip>
ssh-copy-id -f -i /etc/ceph/ceph.pub <your-username>@<backend-server3-ip>
ssh-copy-id -f -i /etc/ceph/ceph.pub <your-username>@<backend-server4-ip>
ssh-copy-id -f -i /etc/ceph/ceph.pub <your-username>@<frontend-server0-ip>
```
Tell Ceph that the new nodes are part of the cluster:
```
sudo ceph orch host add <backend-server2-name> <backend-server2-ip> --labels _admin
sudo ceph orch host add <backend-server3-name> <backend-server3-ip> --labels _admin
sudo ceph orch host add <backend-server4-name> <backend-server4-ip> --labels _admin
sudo ceph orch host add <frontend-server0-name> <frontend-server0-ip> --labels _admin
```
Finally, add storage to the cluster by creating OSDs on all servers:
```
sudo ceph orch daemon add osd <main-backend-server1-name>:/dev/vg1/lv1
sudo ceph orch daemon add osd <backend-server2-name>:/dev/vg1/lv1
sudo ceph orch daemon add osd <backend-server3-name>:/dev/vg1/lv1
sudo ceph orch daemon add osd <backend-server4-name>:/dev/vg1/lv1
sudo ceph orch daemon add osd <frontend-server0-name>:/dev/vg1/lv1
```
Reboot the main host. The Ceph cluster is now up and running.

(doc-install-server-ceph-cluster-deploy)=
### Deploy Ceph Cluster
We are going to use the Ceph cluster as a file system. For the next step we, therefore, deploy [CephFS](https://docs.ceph.com/en/quincy/cephfs/index.html):
```
sudo ceph fs volume create gin-backend
```

Now check the status of your cluster by typing:
```
$ sudo ceph status
  cluster:
    id:     9be8c6b4-74d3-11ed-be0e-005056811b5d
    health: HEALTH_OK

  services:
    mon: 5 daemons, quorum <main-backend-server1-name>,caas-b4bd-x0698,caas-1c47-x0696,caas-dbc9-x0700,caas-9d6b-x0699 (age 27m)
    mgr: <main-backend-server1-name>.iovomf(active, since 27m), standbys: caas-b4bd-x0698.osyhxj
    mds: gin-backend:1 {0=gin-backend.caas-1c47-x0696.ifbdko=up:active} 1 up:standby
    osd: 5 osds: 5 up (since 27m), 5 in (since 27m)

  data:
    pools:   3 pools, 65 pgs
    objects: 22 objects, 2.2 KiB
    usage:   5.0 GiB used, 4.9 TiB / 4.9 TiB avail
    pgs:     65 active+clean
```
You should see a similar output indicating that you have 5 OSDs (object storage devices), 5 daemons, and roughly 5TB of raw data storage available. The actual storage space is only 33% of the raw storage because the stored data is being replicated 3 times (see ```size``` parameter below) by default. It is a good idea to keep this set up which allows 3 out of 5 cluster nodes to fail without affecting the stored data in theory. You can check the parameters of your data storage pool by typing:
```
$sudo ceph osd pool get cephfs.gin-backend.data all
size: 3
min_size: 2
pg_num: 32
pgp_num: 32
crush_rule: replicated_rule
hashpspool: true
nodelete: false
nopgchange: false
nosizechange: false
write_fadvise_dontneed: false
noscrub: false
nodeep-scrub: false
use_gmt_hitset: 1
fast_read: 0
pg_autoscale_mode: on
```

Finally, we are going to mount the newly created gin-backend volume. For that we need to login to the GIN frontend server (Server[0] or the client host). Now generate a minimal conf file for the client host, place it at a standard location, and adjust its permissions:
```
sudo mkdir -p -m 755 /etc/ceph
ssh <your-username>@<main-backend-server1-ip> "sudo ceph config generate-minimal-conf" | sudo tee /etc/ceph/ceph.conf
sudo chmod 644 /etc/ceph/ceph.conf
```
Create a [cephx](https://docs.ceph.com/en/latest/rados/configuration/auth-config-ref/) user (the cephx protocol authenticates Ceph clients and servers to each other) and get its secret key:
```
ssh <your-username>@<main-backend-server1-ip> "sudo ceph fs authorize gin-backend client.<your-username> / rw" | sudo tee /etc/ceph/ceph.client.<your-username>.keyring
sudo chmod 600 /etc/ceph/ceph.client.<your-username>.keyring
```
Perform the actual mounting of CephFS:
```
mkdir /mnt/cephfs
sudo mount -t ceph <any-server-ip>:6789:/ /mnt/cephfs -o name=<your-username>
```
To permanently mount CephFS, open ```/etc/fstab```:
```
sudo nano /etc/fstab
```
Add the following line at the end of the file:
```
<server0-ip>:6789,<server1-ip>:6789,<server2-ip>:6789,<server3-ip>:6789,<server4-ip>:6789:/     /mnt/cephfs    ceph    name=<your-username>,noatime,_netdev    0       0
```
Reboot the node. You are now set for installing GIN on your computing cluster. ```/mnt/cephfs``` folder will act as an interface for GIN's backend storage.

(doc-install-server-docker)=
## Install Docker


(doc-install-server-docker-kvm)=
### Set-up Kernel-based Virtual Machine Support


(doc-install-server-docker-compose)=
### Install Docker Compose
[Docker Compose](https://docs.docker.com/compose/)

```{note}
This section is work in progress. Current priority!
```