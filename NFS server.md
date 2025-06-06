# Network File System (NFS) Server Guide

## **Introduction**
A **Network File System (NFS) Server** allows users to access, share, and manage files over a network as if they were stored locally. Developed by **Sun Microsystems** in 1984, NFS is widely used in UNIX, Linux, and Windows environments.

---

## **1. What is NFS Server?**
An **NFS server** is a machine that provides file access to multiple clients using the **NFS protocol**. This enables seamless file sharing and centralized storage management.

### **How NFS Works**
1. The **NFS Server** exports directories for sharing.
2. The **NFS Client** mounts these directories over the network.
3. Clients access and manipulate files as if stored locally.
4. Any changes made by one client are visible to all others in real-time.

### **Key Features**
-  Remote File Access
-  Centralized Storage
-  Multi-User Access
-  Security and Authentication
-  Scalability
-  Performance Optimization with Caching

---
## **2. Advantages of NFS**
✔ **Cost-Effective** – No additional software required.  
✔ **Seamless Integration** – Works with Linux, UNIX, and Windows.  
✔ **Easy to Configure** – Simple setup and management.  
✔ **Efficient File Sharing** – Reduces redundancy and improves collaboration.  

---

## **3. Disadvantages of NFS**
 **Security Risks** – Improper configuration can expose sensitive data.  
 **Performance Issues** – Can be slow over busy networks.  
 **Reliability** – If the NFS server goes down, all clients lose access.  

---
## **. Setting Up an NFS Server on Linux**

### Check if NFS packages are installed:
```bash
rpm -qa | grep nfs
```

### Install NFS packages:
```bash
yum install nfs-utils
```

### Check information about the NFS package:
```bash
rpm -qi nfs-utils
```

### List files installed by nfs-utils:
```bash
rpm -ql nfs-utils
```

### List documentation for nfs-utils:
```bash
rpm -qd nfs-utils
```

### List configuration files for nfs-utils:
```bash
rpm -qc nfs-utils
```

### Install rpcbind (required for NFS):
```bash
yum install rpcbind
```

### Enable and start the necessary services:
```bash
systemctl enable rpcbind.service
systemctl enable nfs-server.service
systemctl start rpcbind.service
systemctl start nfs-server.service
```

### Create a directory for sharing (e.g., /data):
```bash
mkdir /data
```

### Set permissions to allow full access (read, write, execute):
```bash
chmod 777 /data
```

### Configure NFS exports:
```bash
vim /etc/exports
```

#### Example /etc/exports configuration:
```
/data 192.168.1.0/24(rw,sync)
```

#### Explanation of options:
- **/data**: Shared directory
- **192.168.1.0/24**: Network range of clients that can access the shared directory
- **rw**: Read-write permission for the shared directory
- **sync**: Synchronous writes for better data integrity
- **no_root_squash**: Allows root on the client to have root access on the server (optional)
- **no_all_squash**: Keeps client users' UID and GID intact (optional)

### Export the directory to clients:
```bash
exportfs -rav
```

### Restart NFS server to apply changes:
```bash
systemctl restart nfs-server
```

### Allow NFS-related ports (20048, 2049, 111) in the firewall:
```bash
firewall-cmd --permanent --add-port=2049/tcp
firewall-cmd --permanent --add-port=111/tcp
firewall-cmd --permanent --add-port=20048/tcp
```

### Reloads firewalld to apply the changes:
```bash
firewall-cmd --reload
```

## NFS Client Setup

### Install required packages on the client machine:
```bash
yum install rpcbind nfs-utils
yum install showmount
```

### Check the shared directories on the NFS server:
```bash
showmount -e 192.168.1.10
```

### Create a mount point on the client:
```bash
mkdir /mnt/d1
```

### Mount the NFS share:
```bash
mount -t nfs 192.168.1.10:/data /mnt/d1/
```

### Verify the mount:
```bash
df -h  # Check if the NFS share is mounted successfully
```

### Make the mount persistent by editing /etc/fstab:
```bash
vim /etc/fstab
```

#### Add the following line to the file:
```
192.168.1.10:/data  /mnt/d1  nfs  rw,sync,hard,intr  0  0
```

### To apply changes in /etc/fstab:
```bash
mount -a
```

