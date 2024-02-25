# Table of Content  

[MinIo](#minio)  
[Key Features of MinIO](#key-features-of-minio)  
[Difference between object storage, file storage and block storage](#difference-between-object-storage-file-storage-and-block-storage)  
[Step by step guide to installation and setup](#step-by-step-guide-to-installation-and-setup)  


# MinIO
MinIO is an open-source, high-performance object storage system that is designed for cloud-native applications. It provides an Amazon S3-compatible API, allowing developers to use MinIO as a drop-in replacement for Amazon S3 in their applications. MinIO is often used for storing and retrieving large amounts of unstructured data, such as documents, images, videos, and other types of binary data.

# Key Features of MinIO
**S3 Compatibility**  
MinIO is designed to be fully compatible with the Amazon S3 API. This compatibility ensures that applications and tools built to work with Amazon S3 can seamlessly integrate with MinIO, making it easy for developers to switch between the two without major modifications

**High Performance**  
MinIO is optimized for high performance, providing low-latency and high-throughput access to stored objects. It is designed to handle large-scale data-intensive workloads efficiently.The high performance of MinIO makes it well-suited for use cases where rapid access to large amounts of data is crucial, such as in big data analytics, content delivery networks (CDNs), and other high-performance computing scenarios.   


**Scalability**  
Scalability is a key consideration for organizations dealing with ever-growing datasets. MinIO's ability to scale horizontally provides a cost-effective and efficient way to address increasing storage demands.

# Difference between object storage, file storage and block storage  

**Object storage** is a storage architecture that manages data as objects rather than files or blocks. Object storage is highly scalable and suitable for storing large amounts of unstructured data such as images, videos, documents, backups, and log files.
Examples include Amazon S3, Google Cloud Storage, and Minio.  

**File storage** is a storage system that organizes data into a hierarchical structure of files and directories.  

**Block storage** involves storing data in fixed-size blocks, each with its own address but lacking any metadata or structure.  

## Prerequisites
* Docker or Podman installed
* Read, write, and delete access to the folder or drive used for the persistent volume.

## System Requirement
**Operating System:** Minio supports various operating systems including Linux, macOS, and Windows.   

**CPU:** A multi-core CPU is recommended for better performance, but Minio can run on a single-core CPU.   

**RAM:** At least 4GB of RAM is recommended, but the actual requirement depends on the workload and data size.  

**Disk Space:** The amount of disk space needed depends on the amount of data you plan to store. Minio itself is lightweight, but you'll need enough space for your object data.  

**Network:** A stable network connection is essential for Minio to serve and receive data efficiently.  



# Step by step guide to installation and setup  
If you do not have Docker or Podman installed. You need to install any one of them.
To install Docker Engine, you need the 64-bit version of one of these Ubuntu versions:

* Ubuntu Mantic 23.10  
* Ubuntu Lunar 23.04  
* Ubuntu Jammy 22.04 (LTS)  
* Ubuntu Focal 20.04 (LTS)  

To check bit version run the following command:
```
uname -m
```
Output:  
x86_64  

To check the operating system run the following command:
```
uname
```
Output:  
Linux  

To check the distributor run the following command:
```
cat /etc/os-release
```
Output:  
PRETTY_NAME="Ubuntu 22.04.3 LTS"  
NAME="Ubuntu"  
VERSION_ID="22.04"  
VERSION="22.04.3 LTS (Jammy Jellyfish)"  
VERSION_CODENAME=jammy  
ID=ubuntu  
ID_LIKE=debian  
HOME_URL="https://www.ubuntu.com/"  
SUPPORT_URL="https://help.ubuntu.com/"  
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"  
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"  
UBUNTU_CODENAME=jammy  
## Install Docker
Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

### Step 1: Setup Docker's apt repository
```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
### Step 2: Install the Docker packages
To install the lattest version run:
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
### Step 3: Verification
Verify that the Docker Engine installation is successful by running the hello-world image
```
sudo docker run hello-world
```
Output:  
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
c1ec31eb5944: Pull complete 
Digest: sha256:ac69084025c660510933cca701f615283cdbb3aa0963188770b54c31c8962493
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To check the version of the Docker engine run the following command:
```
docker --version
```
Output:  
Docker version 24.0.7, build afdd53b  

## Start the MinIO container in Docker
Run the following command to start the MinIO container in Docker:
```
mkdir -p ~/minio/data

docker run \
   -p 9000:9000 \
   -p 9090:9090 \
   --name minio \
   -v ~/minio/data:/data \
   -e "MINIO_ROOT_USER=ROOTNAME" \
   -e "MINIO_ROOT_PASSWORD=CHANGEME123" \
   quay.io/minio/minio server /data --console-address ":9090"
```
Explaination of each command:
mkdir -p: This command creates the specified directory and any necessary parent directories. The -p option ensures that it does not report an error if the directory already exists.  

docker run: This command is used to run a Docker container.

-p 9000:9000: Publishes port 9000 on the host to port 9000 on the container. This is the default MinIO server port.

-p 9090:9090: Publishes port 9090 on the host to port 9090 on the container. This is the MinIO console's web interface.

--name minio: Specifies the name of the container as "minio".

-v ~/minio/data:/data: Mounts the ~/minio/data directory on the host to the /data directory inside the container. This is where MinIO will store its data.

-e "MINIO_ROOT_USER=ROOTNAME": Sets the environment variable MINIO_ROOT_USER to "ROOTNAME". This is the username for the MinIO server.

-e "MINIO_ROOT_PASSWORD=CHANGEME123": Sets the environment variable MINIO_ROOT_PASSWORD to "CHANGEME". This is the password for the MinIO server.

quay.io/minio/minio: Specifies the Docker image to be used. In this case, it's the official MinIO server image from Quay.io.

server /data --console-address ":9090": Starts the MinIO server and specifies the data directory (/data) and the console address (:9090).

oUTPUT:
Unable to find image 'quay.io/minio/minio:latest' locally  
latest: Pulling from minio/minio  
f72461870632: Pull complete   
683391db8929: Pull complete   
ba8b8055313f: Pull complete   
a8e0787fb7ed: Pull complete   
fd20cadb8d39: Pull complete   
3738ac54d510: Pull complete   
128c59a31db4: Pull complete   
Digest:   
sha256:5702ea3614203466e8e6616469e460567dc0c82def5a024a90426b25ee4a4d23
Status: Downloaded newer image for quay.io/minio/minio:latest  
Formatting 1st pool, 1 set(s), 1 drives per set.  
WARNING: Host local has more than 0 drives of set. A host failure will result in data becoming unavailable.  
MinIO Object Storage Server  
Copyright: 2015-2023 MinIO, Inc.  
License: GNU AGPLv3 <https://www.gnu.org/licenses/agpl-3.0.html>  
Version: RELEASE.2023-12-20T01-00-02Z (go1.21.5 linux/amd64)  

Status:         1 Online, 0 Offline.   
S3-API: http://172.17.0.2:9000   http://127.0.0.1:9000   
Console: http://172.17.0.2:9090  http://127.0.0.1:9090 

Documentation: https://min.io/docs/minio/linux/index.html  
Warning: The standard parity is set to 0. This can lead to data loss.

### Connect your Browser to the MinIO Server  
Access the MinIO Console by going to a browser and going to http://127.0.0.1:9000 or http://127.0.0.1:9090.

After going to the link you will see the MinIO console:
![image](https://github.com/b02k11/MinIO/assets/132880648/00bfa3ae-1ec9-4b77-9d7c-22d87757f6b8)

### Install MinIO client 
The MinIO Client allows you to work with your MinIO volume from the commandline.  

The following commands add a temporary extension to your system PATH for running the mc utility
```
curl https://dl.min.io/client/mc/release/linux-amd64/mc \
  --create-dirs \
  -o $HOME/minio-binaries/mc

chmod +x $HOME/minio-binaries/mc
export PATH=$PATH:$HOME/minio-binaries/

mc --help
```
The mc alias set command adds or updates an alias to the local mc configuration.
```
mc alias set myminio http://127.0.0.1:9000 {MINIO_ROOT_USER} {MINIO_ROOT_PASSWORD}
```
Replace each argument with the required values.  
ALIAS  
Required The name to associate to the S3-compatible service.

URL  
Required The URL to the S3-compatible service endpoint.   
For example:  

https://minio.example.net


ACCESSKEY      
Required    


The access key for authenticating to the S3 service.  

SECRETKEY  
Required    

The secret key for authenticating to the S3 service.  

Change the {MINIO_ROOT_USER} with your MinIO username and {MINIO_ROOT_PASSWORD} to your MinIO password.  

### Test the connection 
Use the mc admin info command to test the connection to the newly added MinIO deployment:
```
mc admin info myminio  
```
Output:  
```
●  127.0.0.1:9000
   Uptime: 59 minutes 
   Version: 2023-12-20T01:00:02Z
   Network: 1/1 OK 
   Drives: 1/1 OK 
   Pool: 1

Pools:
   1st, Erasure sets: 1, Drives per erasure set: 1

31 KiB Used, 1 Bucket, 2 Objects, 2 Versions
1 drive online, 0 drives offline

```
### Create a bucket in MinIO  
You can create a new bucket by following command:
```
mc mb myminio/test-bucket
```
This command will create a bucker named test-bucket.  
OUTPUT:  
Bucket created successfully `myminio/test-bucket`.
To check the list of bucket use the following command:  
```
mc ls myminio
```
OUTPUT:  
[2023-12-20 19:28:25 IST]     0B sample-bucket/  
[2024-01-04 17:48:58 IST]     0B test-bucket/

### Upload an object/file in bucket  
To upload an object in the bucket run the following command:
```
mc cp /home/brijesh/Documents/Linux_command.md myminio/test-bucket/Linux_command.md
```
mc cp path/to/file myminio/bucketName/objectName  

OUTPUT:  
...command.md: 5.12 KiB / 5.12 KiB ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 475.85 KiB/s 0s  

### The object inside a bucket  
To see the list of objects inside a bucket run the following command:
```
mc ls myminio/test-bucket
```  
OUTPUT:  
[2024-01-04 17:54:33 IST] 5.1KiB STANDARD Linux_command.md  
### Remove object/Bucket     
To remove an object from bucket:  
```
mc rm myminio/test-bucket/Linux_command.md
```  
OUTPUT:  
Removed `myminio/test-bucket/Linux_command.md`.  
To remove multiple object simultaneously   
mc rm myminio/bucketname/objectname1 [objectname2 ...]  

To remove a bucket bucket from MiniIO run the following command:   
```
mc rb myminio/test-bucket
```  
OUTPUT:  
Removed `myminio/test-bucket` successfully.
