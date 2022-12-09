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
$ sudo ceph osd pool get cephfs.gin-backend.data all
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
[Docker Engine](https://docs.docker.com/engine/), [Docker CLI](https://docs.docker.com/engine/reference/run/), and [Docker Compose](https://docs.docker.com/compose/) are dependencies required for GIN installation on a server.

You may already installed all of these Docker components earlier prior to installing Ceph. In that case you can skip this section. If you did not install Docker earlier, the installation instructions for Centos 7 are straight-forward. Just execute the commands below in the terminal on the client node (Server[0]):
```
sudo yum install -y yum-utils
sudo yum-config-manager \
  --add-repo \
  https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```
The final command installs all of the needed Docker components.

(doc-install-server-apache)=
## Install Apache Web Server
Apache web server has to be installed as a prerequisite for GIN installation. GIN is a web app and, as such, it needs a web server software to manage its access. To install Apache, enter the commands below into your terminal:
```
sudo yum install httpd
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload
sudo systemctl start httpd
sudo systemctl status httpd
```

(doc-install-server-git)=
## Install Git and Git-annex
GIN Client installation depends on git and git-annex version cotrol systems. Therefore, it is a good idea to get them sorted before proceeding with the rest of the GIN installation. Installing recent verions of this software on Centos 7 is a bit tricky. This is an example of how to download and install git version control system on your client node (Server[0]):
```
sudo yum install epel-release
sudo yum install https://repo.ius.io/ius-release-el$(rpm -E '%{rhel}').rpm
sudo yum install yum-plugin-replace
sudo yum install git
sudo yum replace git --replace-with git236
```

To download and install git-annex version control system, follow these steps:
```
sudo yum install wget
wget https://downloads.kitenet.net/git-annex/linux/current/rpms/git-annex-standalone-10.20221004-1.x86_64.rpm
sudo yum localinstall git-annex-standalone-10.20221004-1.x86_64.rpm
rm git-annex-standalone-10.20221004-1.x86_64.rpm
```

(doc-install-server-gin)=
## Install GIN
GIN full server instalation instructions can be found [here](https://gin.g-node.org/G-Node/in-house-gin#deploy-and-run-a-full-server-using-docker-compose). We are going to go step by step through these installations adapted for our particular case.

(doc-install-server-gin-client)=
### Install GIN client
Download and install GIN client, so that you are able to download public GIN repositories needed for GIN server installation:
```
curl --silent --remote-name --location https://gin.g-node.org/G-Node/gin-cli-releases/raw/master/gin-cli-latest-linux.tar.gz
tar -xvf gin-cli-latest-linux.tar.gz
sudo mv gin /usr/bin/
rm README.md
rm gin-cli-latest-linux.tar.gz
```

(doc-install-server-gin-fodlers)=
### Set-up GIN Installation Folders
We need to set up certain user groups and users, as well as a particular directory structure in order to run GIN on Docker. The bash script file that will do exactly that is provided below:
```bash
#!/bin/bash

# Provide the user that will use docker-compose to start the service
DEPLOY_USER=<your-username-string>

# Specify the GIN files root location
DIR_GINROOT=<root-dir-path-string>

# Postgres database password
PGRES_PASS=<pgres-password-string>

# Prepare a ginservice group if it does not exist
if [ $(getent group ginservice) ]; then
  echo "group ginservice already exists."
else
  groupadd ginservice
fi

# Create dedicated "ginuser" user and add it to the "docker"
# group; and the "ginservice" group; create all required 
# directories with this user and the "ginservice" group permission.
if [ $(getent passwd ginuser) ]; then
    echo "user ginuser already exists"
else
    useradd -M -G docker,ginservice ginuser
    # Disable login for user ginuser
    usermod -L ginuser
fi

# Make sure to add the user running docker-compose to the ginservice group as well!
usermod -a -G ginservice $DEPLOY_USER

# Make sure that required directories are empty
rm -rf $DIR_GINROOT

# Required directories
DIR_GINCONFIG=$DIR_GINROOT/config
DIR_GINVOLUMES=$DIR_GINROOT/volumes
DIR_GINDATA=$DIR_GINROOT/gindata

# Create gin specific directories
# The 'notice' directory may contain a banner.md file.
# The content of this file is displayed on the GIN page without a service restart e.g.
#  to inform users of an upcoming service downtime.
mkdir -vp $DIR_GINCONFIG/gogs/notice
mkdir -vp $DIR_GINCONFIG/postgres

mkdir -vp $DIR_GINVOLUMES/ginweb

mkdir -vp $DIR_GINDATA/gin-repositories
mkdir -vp $DIR_GINDATA/gin-postgresdb

mkdir -vp $DIR_GINROOT/gin-dockerfile

# Create an env file to specify the docker compose project name
echo "COMPOSE_PROJECT_NAME=gin" > $DIR_GINROOT/gin-dockerfile/.env

# Create postgres secret file
echo "POSTGRES_PASSWORD=${PGRES_PASS}" > $DIR_GINCONFIG/postgres/pgressecrets.env

# make sure an empty gogs config file is available; technically only required for previous versions of gogs
touch $DIR_GINCONFIG/gogs/conf/app.ini
```
Open a bash script file in your home directory, for example:
```
sudo nano gin-docker-folders.sh
```
Paste the contents of the file as shown above, save, and close the file. You have to provide only three parameters in this file: your username, password, and GIN installation directory. Execute this file by typing:
```
sudo sh gin-docker-folders.sh
```

(doc-install-server-gin-instance-file)=
### Prepare GIN Instance File
We need to prepare the GIN Docker Compose instance file. First, let's organise all GIN resources in a single folder:
```
mkdir in-house-gin-resources
mv gin-docker-folders.sh in-house-gin-resources/
cd in-house-gin-resources
git clone https://gin.g-node.org/G-Node/in-house-gin.git
mv in-house-gin in-house-gin-repo
```
Now copy, open, and prepare the actual file:
```
cp in-house-gin-repo/resources/docker-compose.yml docker-compose.yml
sudo nano docker-compose.yml
```
You need to adjust ```PUID``` which is the ```ginuser``` user ID. You can find it by typing:
```
$ sudo cat /etc/passwd
```
```
mark:x:1001:1001:mark,,,:/home/mark:/bin/bash
[--] - [--] [--] [-----] [--------] [--------]
|    |   |    |     |         |        |
|    |   |    |     |         |        +-> 7. Login shell
|    |   |    |     |         +----------> 6. Home directory
|    |   |    |     +--------------------> 5. GECOS
|    |   |    +--------------------------> 4. GID
|    |   +-------------------------------> 3. UID
|    +-----------------------------------> 2. Password
+----------------------------------------> 1. Username
```
Locate the ```ginuser``` line and pick the third entry (UID).

PGID (```ginservice``` group ID) is another entry that you need to specify. The following command will help you to find it:
```
sudo cat /etc/group
```
Simlarly as before, locate the line starting with ```ginservice``` and pick the third entry.

Next, make sure the set ```ipv4_address``` matches the IP set in your apache webserver configuration. Type in
```
ifconfig -a
```
```eth0:inet``` entry will give you your IP address. Enter this value into the ```docker-compose.yml``` file.

```{note}
This section is work in progress. Current priority!
```