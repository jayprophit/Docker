# Complete Docker Course - From Beginner to Pro(Learn Containers)

## Prerequisites
  - Familiarity with web applications (the course examples are in JavaScript + Golang)
  - Basic shell commands (enough to build/ run an application in a language of your own choosing on Linux

## Course Overview
1. History and Motivation
2. Technology Overview
   1. Containers
   2. Docker
3. Installation/ Set up & hello World
4. Using 3rd party containers
5. Demo Application
6. Building Container Images
   1. Dockerfiles Basics
   2. Dockerfile Optimization
   3. Build + multi-architecture
7. Container Resitries
8. Running Containers
9. Container Security
10. Interacting with Docker Objects
11. Development Workflow
12. Deploying Containers


### Demo Application
Minimal 3 Tier web Application
   - React Frint End
   - Two API implementations:
      - node.js (interpreted)
      - golang (complied)
   - PostgresSQL Databas
Deployed to:
   - Docker Swarm
   - Railway.app (Seperate material)
   - Kubernetes (Seperate material)


## 01. History and Motivation
- It works on my machine
- then we'll ship your machine
- and thats how Docker was born

|Motivation for Containers Develoment|Deployment|
|----------|----------|
|"To get the development enviroment set up install Postgres, MongoDB, and run these 5 scripts.  Oh wait, you are on Windows? Also change these configurations"|"To deploy the application, provision a server running Ubuntu, run this Ansible playbook to install the dependencies and configure the system, then copy the deployment binary and run it with these options"|
|⬇️|⬇️|
|"run docker compose up"|"Run this container image with these options"|

### What is a container?
A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application

e.g.
1. Docker
  1. SQL Alchemy
  2. FastAPI
  3. Python
  4. Alpine Linux
  5. etc

Its a standard image that you can use to run multiple copies of it.

#### container image
is an artifact that has all of the dependencies within it.

#### container
is what you run from that image.

like with (OOP) Object Oriented Programming a container image is like a class, and the container like an instance (instantiation) of that class.  A container is a standardized packaging, which we can create one or more copies, the copies are exact duplicate (copy) each time.

|Open Container Initiative (OCI)|The Open Container Initiative is an open governance structure for the express purpose of creating open industry standards around container formats and runtimes.|
|----------|----------|
|- Runtime Specification|how to take an image that adheres to that image specification and run it in a container|
|- Image Specification  |defines what should be included in the image, in terms of metada & format - a serializable file system|
|- Distibution Specification|talks about how those images should be distributed, like registries - pushing and pulliing of those images|

### Evolution of Virtualization
#### Bare Metal - (computing)
<table>
  <tr>
    <th colspan="2">Host (physical) Machine</th>
  </tr>
  <tr>
    <th>Application #1</th>
    <th>Application #2</th>
  </tr>
  <tr>
    <td colspan="2">Binaries/ Libraries</td>
  </tr>
  <tr>
    <td colspan="2">Operating System</td>
  </tr>
  <tr>
    <td colspan="2">Physical Hardware</td>
  </tr>
</table>

  - Hellish dependency conflits
  - Low utilization efficiency
  - Large blast radius
  - Slow start up & shut down speed (minutes)
  - Very slow provisioning & decommissioning (hours to days)

#### Virtual Machine - (VM)
<table>
  <tr>
    <th colspan="2">Type 1: Aws nitro System, VMWARE vSphere, Mirosoft Hyper-V</th>
  </tr>
  <tr>
    <td colspan="2">Type 2: VirtualBox</td>
  </tr>
</table>

<table>
  <tr>
    <th colspan="2">Host (physical) Machine</th>
  </tr>
  <tr>
    <th>Virtual Machine #1</th>
    <th>Virtual Machine #2</th>
  </tr>
  <tr>
    <td>Application #1</td>
    <td>Application #2</td>
  </tr>
  <tr>
    <td>Binaries/ Libraries</td>
    <td>Binaries/ Libraries</td>
  </tr>
  <tr>
    <td>Operating System</td>
    <td>Operating System</td>
  </tr>
  <tr>
    <td>Virtual Hardware</td>
    <td>Virtual Hardware</td>
  </tr>
  <tr>
    <td colspan="2">Hypervisor</td>
  </tr>
  <tr>
    <td colspan="2">Operating System (if "type 2" hyperviosr</td>
  </tr>
  <tr>
    <td colspan="2">Physical Hardware</td>
  </tr>
</table>

  - No dependency conflicts
  - Better utilization efficiency
  - Small blast radius
  - Faster startup and shutdown (minutes)
  - Faster provisioning & decommissioning (minutes)

**Note:** There are a new class of micro-VMs designd to start in seconds! (research "Firecracker VM" for more info!)


#### Containers
<table>
  <tr>
    <th colspan="2">Desktop Container Platforms: docker, podman</th>
  </tr>
  <tr>
    <td colspan="2">Container Runtimes: containerd, cri-o</td>
  </tr>
</table>

<table>
  <tr>
    <th colspan="2">Host (virtual or physical) Machine</th>
  </tr>
  <tr>
    <th>Container #1</th>
    <th>Container #2</th>
  </tr>
  <tr>
    <td>Application #1</td>
    <td>Application #2</td>
  </tr>
  <tr>
    <td>Binaries/ Libraries</td>
    <td>Binaries/ Libraries</td>
  </tr>
  <tr>
    <td colspan="2">Container Runtime</td>
  </tr>
  <tr>
    <td colspan="2">Operating System</td>
  </tr>
  <tr>
    <td colspan="2">(Virtual or Physical) Hardware</td>
  </tr>
</table>

  - No dependency conflicts
  - Even better utilization efficiency
  - Small blast Radius
  - Even faster startup and shutdown (seconds)
  - Lightweight enough to use in development


#### Virtual Machines + Containers + Orchestrators!
<table>
  <tr>
    <th">Orchestrators Systems: Kubernetes, Nomad - hashicorp, Docker swarm</th>
  </tr>
</table>

<table>
  <tr>
    <th colspan="4">Host (physical) Machine</th>
  </tr>
  <tr>
    <th colspan="2">Virtual Machine #1</th>
    <th colspan="2">Virtual Machine #2</th>
  </tr>
  <tr>
    <th>Container #1</th>
    <th>Container #2</th>
    <th>Container #3</th>
    <th>Container #4</th>
  </tr>
  <tr>
    <td>Application #1</td>
    <td>Application #2</td>
    <td>Application #3</td>
    <td>Application #4</td>
  </tr>
  <tr>
    <td>Binaries/ Libraries</td>
    <td>Binaries/ Libraries</td>
    <td>Binaries/ Libraries</td>
    <td>Binaries/ Libraries</td>
  </tr>
  <tr>
    <td colspan="2">Container Runtime</td>
    <td colspan="2">Container Runtime</td>
  </tr>
  <tr>
    <td colspan="2">Operating System</td>
    <td colspan="2">Operating System</td>
  </tr>
  <tr>
    <td colspan="2">Virtual Hardware</td>
    <td colspan="2">Virtual Hardware</td>
  </tr>
  <tr>
    <td colspan="2">Hypervisor</td>
  </tr>
  <tr>
    <td colspan="2">Physical Hardware</td>
  </tr>
</table>


#### Tradeoffs
<table>
  <tr>
    <th></th>
    <th style="text-align: center;">Bare Metal</th>
    <th style="text-align: center;">Virtual Machine</th>
    <th style="text-align: center;">Container</th>
  </tr>
  <tr>
    <th style="text-align: center;">Dependency Management</th>
    <td style="background-color: red;"></td>
    <td style="background-color: green;"></td>
    <td style="background-color: green;"></td>
  </tr>
  <tr>
    <th style="text-align: center;">Utilization</th>
    <td style="background-color: red;"></td>
    <td style="background-color: yellow;"></td>
    <td style="background-color: green;"></td>
  </tr>
    <tr>
    <th style="text-align: center;">Isolation</th>
    <td style="background-color: green;"></td>
    <td style="background-color: green;"></td>
    <td style="background-color: yellow;"></td>
  </tr>
      <tr>
    <th style="text-align: center;">Start Up Speed</th>
    <td style="background-color: red;"></td>
    <td style="background-color: yellow;"></td>
    <td style="background-color: green;"></td>
  </tr>
    <tr>
    <th style="text-align: center;">Dev/ Prod Parity</th>
    <td style="background-color: red;"></td>
    <td style="background-color: yellow;"></td>
    <td style="background-color: green;"></td>
  </tr>
      <tr>
    <th style="text-align: center;">Control</th>
    <td style="background-color: green;"></td>
    <td style="background-color: yellow;"></td>
    <td style="background-color: yellow;"></td>
  </tr>
    <tr>
    <th style="text-align: center;">Performance</th>
    <td style="background-color: green;">See note</td>
    <td style="background-color: green;">See note</td>
    <td style="background-color: green;">See note</td>
  </tr>
      <tr>
    <th style="text-align: center;">Operational Overhead</th>
    <td style="background-color: red;"></td>
    <td style="background-color: yellow;"></td>
    <td style="background-color: green;"></td>
  </tr>
  </table>

**Note:** There is much more nuance to "performance" than this chart can capture. A VM or container dosen't inherently sacrice much performance relative to bare metal it runs on, but being able to have more controlover things like connected storage, physical proximity of the system relative to others it communicates with, specific hardware accelerators, etc... do enable performance tuning.

## 02. Technology Overview
- CGROUPS AND NAMESPACES!!!

### Linux Building Blocks
<table>
<tr>
  <td>Name outside of their own instance spaces</td>
  <td>Control Groups (cgroups)</td>
  <td>Union Filesystem</td>
</tr>
</table>

#### Namespaces
"A namespace wraps a global system resource imn an abstraction that makes it appear to the process within the namespace that they have their own isolated instance of the global resource.
  
Chnages to the global resource are visible to other processes that are members of the namespace, but are invisible to other processes."

<table>
  <tr>
    <th>Namespace</th>
    <th>Flag</th>
    <th>Page</th>
    <th>Isolates</th>
  </tr>
  <tr>
    <td>Cgroup</td>
    <td>CLONE_NEWCGROUP</td>
    <td>cgroup_namespaces(7)</td>
    <td>cgroup root directory</td>
  </tr>
  <tr>
    <td>IPC</td>
    <td>CLONE_NEWIPC</td>
    <td>ICP_namespaces(7)</td>
    <td>System V IPC, POSIX message queues</td>
  </tr>
  <tr>
    <td>Network</td>
    <td>CLONE_NEWNET</td>
    <td>network_namespaces(7)</td>
    <td>Network devices, stacks, ports, etc</td>
  </tr>
  <tr>
    <td>Mount</td>
    <td>CLONE_NEWNS</td>
    <td>mount_namespaces(7)</td>
    <td>Moint points</td>
  </tr>
  <tr>
    <td>PID</td>
    <td>CLONE_NEWPID</td>
    <td>pid_namespaces(7)</td>
    <td>Process IDs</td>
  </tr>
  <tr>
    <td>Time</td>
    <td>CLONE_NEWTIME</td>
    <td>time_namespaces(7)</td>
    <td>Boot and monotonic clocks</td>
  </tr>
  <tr>
    <td>User</td>
    <td>CLONE_NEWUSER</td>
    <td>user_namespaces(7)</td>
    <td>User and group IDs</td>
  </tr>
  <tr>
    <td>UTS</td>
    <td>CLONE_NEWUTS</td>
    <td>uts_namespaces(7)</td>
    <td></td>
  </tr>
</table>

### Control Groups (cgroups)
"a Linux feature which allows process to be organized into hierarchical groups whose usage of various types of resources can then be limited and monitored"

<table>
  <tr>
    <th>Application A</th>
    <td>Use up to 30% of CPU cycles (cpu.shares)</td>
    <td>Use up to 50 MB Memory (memory.limit_in_bytes)</td>
    <td>Throttle reads to 5 MB/s (blxio.throttleread_bps_device)</td>
</tr>
  <tr>
    <th>Application B</th>
    <td>Use up to 40% of CPU cycles (cpu.shares)</td>
    <td>Use up to 100 MB Memory (memory.limit_in_bytes)</td>
    <td>Throttle reads to 10 MB/s (blxio.throttleread_bps_device)</td>
  </tr>
</table>

### Union Mount Filesystem (overlayfs)
"allows files and directories of seperate file systems, known as branches, to be transparently overlaid, forming a single coherent file system.

Contents of direcotries which have the same path within the merged branches will be seen together in a single merged directory, within the new, virtual silesystem"

<table> 
  <tr>
    <th>Overlay</th>
    <td>file-1</td>
    <td>file-2b</td>
    <td>file-3</td>
    <td></td>
  </tr>
  <tr>
    <th>Upper</th>
    <td></td>
    <td>file-2b</td>
    <td>file-3</td>
    <td>file-4 whiteout</td>
  </tr>
  <tr>
    <th>Lower</th>
    <td>file-1</td>
    <td>file-2a</td>
    <td></td>
    <td>file-4</td>
  </tr>
</table>

### Docker Desktop Architecture
<table>
   <tr>
      <th colspan="6">Docker Desktop</th>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td colspan="3">Linux Virtual Machine</td>
      <td></td>
   </tr>
   <tr>
      <td colspan="2">client</td>
      <td colspan="3">Server/ Host</td>
      <td>Registry (e.g. Dockerhub)</td>
   </tr>
   <tr>
      <td></td>
      <td style="background-color: orange;">Docker CLI (docker)</td>
      <td></td>
      <td style="background-color: orange;", colspan="2">Docker API</td>
      <td>My Container Image 1</td>
   </tr>   
   <tr>
      <td></td>
      <td>Graphical User Interface</td>
      <td></td>
      <td style="background-color: orange;", colspan="2">Daemon (dockerd)</td>
      <td>My Container image 2</td>
   </tr>   
   <tr>
      <td></td>
      <td>Docker Credntial helpers</td>
      <td></td>
      <td>Container 1a</td>
      <td>My container image 1</td>
      <td>nginx</td>
   </tr>
   
   <tr>
      <td></td>
      <td>Extensions</td>
      <td></td>
      <td>Container 1b</td>
      <td>My container image 2</td>
      <td>Ubuntu</td>
   </tr>
   
   <tr>
      <td></td>
      <td></td>
      <td colspan="3">K8s Cluster (kubeadm) Kubernetes</td>
      <td style="background-color: orange;">Part of "docker Engine" (open Source)</td>
   </tr>
</table>

## 03. Installation/ Set up & hello World
<table>
  <tr>
    <th>Personal</th>
    <th>Pro</th>
    <th>Team</th>
    <th>Business</th>
  </tr>
  <tr>
    <th>$0</th>
    <th>$5/ pm</th>
    <th>$9/ per user/ pm</th>
    <th>$24/ per user/ pm</th>
  </tr>
</table>

**Note:** prices may very

### Installation
[Docker Desktop](https://docs.docker.com/get-docker/)

<table>
  <tr>
    <th>Docker Desktop for Mac</th>
    <th>Docker Desktop for Windows</th>
    <th>Docker Desktop for Linux</th>
  </tr>
</table>

[Docker Engine](https://get.docker.com/)

Alternative Youtube Tutorials by **Bret Fisher** & **Nuno do Carmo** for a deep dive on Docker Desktop on windows 11.

In Docker's IDE under settings option.  you can set how many of your systems resouces that docker has access to.  

### 1st program image

```markdown
**Terminal:** docker run docker/whalesay cowsay "👋 Hey Team"
```

### A copy of Postgres version 15.1 running on the Alpine operating system for ruuning our application
**Note: a password needs to be set or it want run**

As were are running on an isolated network, so to connect from a host, we are also publishing on **Port:5432** 

set the version we want to use **Alpine**

```markdown
**Terminal:** docker run -- env POSTGES_PASSWORD=foobarbaz --publish 5432:5432 postgres:15-alpine
```

To check that its working we can check it in 
```markdown
**pgadmin**
```

you can get all the tables from that information schema within the database running inside the container by using this command:

```markdown
**run:** SELECT * FROM information_schema.tables
```

## 04. Using 3rd party containers
  - I made this
  - You made this?
  - **Container images on DockerHub**
  - I made this
  - **Container running in production**

### DockerHub has 100k+ Public images!
is a container registry hosted by the company docker withover 100k+ images, these range from alpine and ubuntu which are different varieties of Linux, tools like Engine X  which is a web server & reverse proxi or utilitiy images like busybox which has tones of useful commands

[DockerHub](https://hub.docker.com)

### 01 Understanding Data within Containers

  - 🚨**WARNING**🚨: By default all data created or modified in containers is ephemeral

### Understanding data persistance
when we create a container from a container image, everuthing in the image is treated as read-only, and there is a new layer overlayed on top that is read/write

#### Container Filesystem
<table>
  <tr>
    <th colspan="3"></th>
  </tr>
  <tr>
    <th>Container Layer</th>
    <td></td>
    <td>R/W</td>
  </tr>
  <tr>
    <th>Build Layer 3</th>
    <td>Run npm install</td>
    <td>RO</td>
  </tr>
  <tr>
    <th>Build Layer 2</th>
    <td>Copy...</td>
    <td>RO</td>
  </tr>
  <tr>
    <th>Build Layer 1</th>
    <td>Run opt update && opt install node.js</td>
    <td>RO</td>
  </tr>
  <tr>
    <th>Base Image</th>
    <td>ubuntu:22.04</td>
    <td>RO</td>
  </tr>
</table>

  - If some data should be present everytime a container image is run (e.g. dependency), it should be built into an image itself

```markdown
**Note:**
 R/W = read/ write
 RO = read only
-c = container
ls = list services
```

#### A. Installing Dependecies:
Let's experiment with how installing into a container at runtime behaves!

**Note:** Modifiying the content of a container at runtime is not something you would normally do.  We are doing it here for instructional purposes only!

```markdown
# Create a container from ubuntu image
docker run --interactive --tty --rm ubuntu:22.04

# Try to ping google.com
ping google.com -c 1 # This results in 'bash: ping: command not found

# install ping
apt update
apt install iputils-ping --yes

ping google.com - c 1 # this time it succeeds!

```
#### --interactive --tty
these 2 flags together means that when we issuse this command, it will give us a runnning shell within that container

#### --rm
once we exit it should not store that stop container but it should remove that from our system

let's try that again:
```markdown
**Terminal:** docker run -it --rm ubuntu:22.04
ping google.com -c 1 # It fails
```

It fails the second time because we installed it into that read/ write layer specific to the first container, and when we tried again it was a seperate containter with a seperate read/ write

We can give the container a name so that we can twell docker to reuse it:

```markdown
# Create a container from ubuntu image (with a name and WITHOUT the --rm flag)
docker run --it --name my-ubuntu-container ubuntu:22.04

# Install & use ping
apt update
apt install iputils-ping --yes
ping google.com -c 1
exit

# List all containers
docker container ps -a | grep my-ubuntu-container
docker container inspect my-ubuntu-container

# Restart the container and attach to running shell
docker start my-ubuntu-container
docker attach my-ubuntu-container

# Test ping
google.com -c 1 # It should now succeed! 🍕
```

naming the conainter: **--name my-ubuntu-container**

```markdown
**Terminal:** docker run --interactiv --tty --name my-ubuntu-container ubuntu:22.04
```

It will only show a short version of a list as its in a stopped state.  To list the container type:
```markdown
**Terminal:** docker ps
```

to show details of the list with a status.  To list the container with a status type:
```markdown
**Terminal:** docker ps -a
```

We want the image to always be there when we start a container from that perticular image. So generally we never want to rely on a container to persist the data, so for a dependency like this we would want to include it in the image:

```markdown
# Create a container from ubuntu image as base with ping installed
docker build --tag my-ubuntu-container -<<EOF
From ubuntu:22.04
RUN apt update && apt install iputils-ping --yes

# Run a container based on this image
docker run -it --rm my-ubuntu-image

# Confirm that ping was pre-installed
ping google.com -c 1 # Success!😊
```

The FROM... RUN... stuff is part of what is called a Dockerfile that is used to specify how to build a container image.  We will go much deeper into building containers later in the course, but for now just understand that for anything we need in the container at runtime we should build into the image!

The one exception to this rule is the environment specific configuration (environment variables, config files, etc) which can be provided at runtime as a part of the environment (see: https://12factor.net/config)

#### B. Persisting Data by the Application
Often, our application produce data that we need to safely persist (e.g. database data, user upload data, etc...) even if the containers are destroyed `nd recreated.  Luckily Docker (and containers more generally) have a feature to handle this use case called **Volumes** and **Mounts**!

#### Host
<table>
   <tr>
    <th colspan="4">Host</th>
  </tr> 
  <tr>
    <th colspan="2"></th>
    <td colspan="2">Docker Desktop VM</td>
  </tr>
  <tr>
    <td colspan="2">Bind Mount</td>
    <td colspan="2">Volume Mount</td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td>Container</td>
    <td></td>
  </tr>  
  <tr>
    <td colspan="2">container</td>
    <td colspan="2">Volume Mount</td>
  </tr>
  <tr>
    <td colspan="2">Host Filesystem/ path/ to/ my/ data</td>
    <td colspan="2">Docker Area/ var/ lib/ docker/ volumes/</td>
  </tr>
</table>

  - If data is generated by the application that needs to be persisted, a Volume should be used to store that outside of the ephemerl container filesystem

Volumes and mounts allow us to specify a location where data should persist beyound the lifecycle of a single container.  The data can live in a location managed by Docker (volume mount), a location in your host filesystem (bind mount), or in memory (tmpfs mount, not pictured)

**Note:** this third option (mpfs mount) dose not persist the data after the container exits, and instead used as tepory store for data you specifically DON'T want to persist (for example credential files).  It is included here for completness but should not be used for application data you want to persist.

lets experiment with how creating some data within a container at runtime behaves!

```markdown
**Note:**
  mkdir = make directory
  echo = call data
  cat = concatenate
  -v = volume
```

```markdown
# Create a container from ubuntu image
docker build --it --rm ubuntu:22.04

# Make a directory and store a file in it
mkdir my-data
echo "Hello from container!" > /my-data/hello.txt

# Confirm the file exists
cat my-data/hello.txt
exit
```

If we then create a new container, (as expected) the file dose not exist!

```markdown
# Create a container from ubuntu image
docker build --it --rm ubuntu:22.04

# check if my file exists
cat my-data-/hello.txt
# Produces error: 'cat: my-data/hello.txt: No such file or directory
```

#### 01. Volume Mounts
We can use volumes and mounts to safely persist the data

```markdown
# Create a named volume
docker volume create my-volume

# Create a container and mount the volume into the container filesystem
docker run -it --mount source=my-volume,destination=/my-data/ ubuntu:22.04

# There is a similar (but shorter) syntax using -v which accomplishes the same
docker run -it --rm -v my-volume:/my-data ubuntu:22.04

# Now we can create and store the file into the location we mounted the volume
echo "Hello from the container!" > /my-data/hello.txt
cat my-data/hello.txt
exit
```

We can now create a new container and mount the existing volume to confirm the file persisted:

```markdown
# Create a new container and mount the volume into the container folesystem
docker build --it --rm --mount source=my-volume,destination=/my-data/ ubuntu:22.04

cat my-data/hello.txt
# This time it succeeds!
exit
```

Where is this data located?  On linux it would be at /var/lib/docker/volumes... but remember, on docker desktop, Docker runs linux virtual machine.

One way we can view the filesytem of that VM is to use a **container image** created by **justincormat** that allows us to create a container within the namespace of PID 1.  This effectively gives us a container with root access in that VM.

**NOTE:** Generally you should be careful running containers in privileged mode with access to the host system in this way.  Only do it if you have a specific reason to do so and trust the contaner image.

```markdown
# Create a container that can access the Docker Linux VM
# Pinning to the image hash ensures it is this SPECIFIC image and not an updated one helps minimize the potential of a supply chain attact
docker build --it --rm privileged --pid=host justincormack/ nsenter1@sha256:5af0be5e42ebd55eea2c593e4622f810065c3f45bb805eaacf43f08f3d06ffd8

# Navigate to the volume inside the VM at:
ls /var/lib/docker/volume/my-olume/_data
cat /var/lib/volume/my-volume/_data/hello.txt
# Woohoo! we found our data
```

This approach can then be used to mount a volume at the known path where a program persists its data:

```markdown
**NOTE:**
  pgdata = postgre data
  PWD = present working directory
```


```markdown
# Create a container from postgre container image and mount its known storage path into volume named pgdata
docker run -it --rm -v pgdata:/var/lib/postgresql/data -e POSTGRES_PASSWORD=foobarbaz postgres:15.1-alpine
```

#### 02. Bind Moounts
Alternatively, we can mount a directory from the host system using bind mount:

```markdown
# Create a container that mounts a directory from the host filesystem into the container
docker run -it --rm --mount type=bind,source="${PWD}"/my-data,destination=/my-data ubuntu:22.04

# Again, there is a similar (but shorter) syntax using -v which accomplishes the same
docker run -it --rm -v ${PWD}/my-data:/my-data ubuntu:22.04

echo "Hello from the container!" >/my-data/hello.txt

# You should also be able to see the hello.txt file on your host system
cat my-data/hello.txt
exit
```


Bind mounts can be nice if you want easy visibility into the data being stored, but there are a number of reasons outlined at [Docker storage - volumes](https://docs.docker.com/storage/volumes/) (including speed if you are running Docker Desktop on windows/ mac) for why volumes are prefered.

### 02. Use Cases
Now that we have an understanding of how data storage works with containers we can start to explore various cases for running 3rd party containers.

For me, the main catergories are database, interactive test environments, and CLI utilities.

### A. Database
Database are notoriously fickle to install and configure.  The instructions are often complex and vary different versions and operating systems.  For development, where you might need to run multiple versions of a single database or create a freash database for testing purpose running in a container can be a massive improvement.

The setup/installation is handled by the container image, and all you need to provide is some configuration values.  Switching between verions of the database is as easy as specifying a different image tag (e.g. **postgres:14.6** vs **postgre:15.1**).

A few key considerations when running database in containers:

  - **Use volume(s) to persist data:** The entire reason for the section above was to give you an understanding of how to avoid data loss.  Generally databases will store its data at one or more known paths.  You should identify those and mount volumes to those locations in the containers to ensure data persists beyound the container.

  - **Use bind mount(s) for additional config:** Often databases use configuration files to influenece runtime behavior.  You can create these files on your host system, and then use bind mount to place them in the correct location within the container to be read upon startup.

  - **Set environment varibles:** In addition to configuration files many databases use environment varibles to influence runtime behavior.  (example setting the admin password).  Identify these varibles and set them accordingly.

Here are some useful database container images and sample commands that attempt to mount the necessary data directories into volumes and set key environment variables.

🚨🚨🚨**WARNING:** While i have made a best effort to set up the volume mounts properly, please confirm the volume mounts match the location data is persisted within the container independently to ensure your data safely.🚨🚨🚨

#### POSTGRES
[Docker Postgres](https:hub.docker.com/_/postgres)

```markdown
docker run -d --rm \
  -v pgdata:/var/lib/postgresql/data \
  -e POSTGRES_PASSWORD=foobarbaz \
  -p 5432:5432 \
  postgres:15.1-alpine

# With custom postgresql.conf file
docker run -d --rm \
  -v pgdata:/var/lib/postgresql/data \
  - ${PWD}/postgres.conf:/etc/postgresql/postgresql.conf \
  -e POSTGRES_PASSWORD=foobarbaz \
  -p 5432:5432 \
  postgres:15.1-alpine -c 'config_file=/etc/postgresql/postgresql.conf'
```

#### MONGO

[Docker Mongo DB](https://hub.docker.com/_/mongo)

```markdown
docker run -d --rm \
  -v mongo:/data/db \
  -e MONGO_INITDB_ROOT_USERNAME=root \
  -e MONGO_INITDB_ROOT_PASSWORD=foobarbaz \
  -p 27017:27017 \
  mongo:6.0.4

# With custom mongo.conf file
docker run -d --rm \
  -v mongo:/data/db \
  -v ${PWD}/postgres.conf:/etc/postgresql
  -e MONGO_INITDB_ROOT_USERNAME=root \
  -e MONGO_INITDB_ROOT_PASSWORD=foobarbaz \
  -p 27017:27017 \
  mongo:6.0.4 --config /etc/mongod.conf
```

#### Redis

[Docker Redis](https://hub.docker.com/_/redis)

Depending how you are using redis within your application, you may or may not care if the data is persisted.

```markdown
docker run -d --rm \
  -v resdata:/data \
  redis:7.0.8-alpine

# With custom redis.conf file
docker run -d --rm \
  -v redisdata:/data \
  -v ${PWD}/redis.conf:/usr/local/etc/redis/redis.conf \
  redis:7.0.8-alpine redis-server /usr/local/etc/redis.conf
```

#### MySQL

[Docker MySQL](https://hub.docker.com/_/mysql)

```markdown
docker run -d --rm \
  -v mysqldata:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=foobarbaz \
  mysql:8.0.32

# With custom conf.d
docker run -d --rm \
  -v mysqldata:/var/lib/mysql \
  -v ${PWD}/conf.d:/etc/mysql/conf.d \
  -e MYSQL_ROOT_PASSWORD=foobarbaz \
  mysql:8.0.32
```

#### Elasticsearch

[Docker elasticsearch](https://hub.docker.com/_/elasticsearch)

```markdown
docker run -d --rm \
  -v elasticsearchdata:/usr/share/elasticsearch \
  -e ELASTICSEARCH_PASSWORD=foobarbaz \
  -e "discovery.type=single-node" \
  -p 9200:9200 \
  -p 9300:9300 \
  elasticsearch:8.0.0
```

#### Neo4j

[Docker Neo4j](https://hub.docker.com/_/neo4j)

```markdown
docker run -d --rm \
  -v=neo4jdata:/data \
  -e NEO4J_AUTH=neo4j/foobarbaz \
  -p 7474:7474 \
  -p 7687:7687 \
  noe4j:5.4.0-community
```

### B. Interactive Test Environments

#### 01. Operating Systems

````markdown
# https:.docker.com/_/ubuntu
docker run -it --rm ubuntu22:04

# https://.docker.com/_/debian
docker run -it --rm debian:bulllseyee-slim

# https://.docker.com/_/alpine
docker run -it --rm alpine:3.17.1

# https://.docker.com/_/buusybox
docker run -it --rm busybox:1.36.0
# small image with lots of useful utilities
````

#### 02. Programming runtimes:

````markdown
# https:.docker.com/_/Python
docker run -it --rm python

# https://.docker.com/_/node
docker run -it --rm node:18.13.0

# https://.docker.com/_/php
docker run -it --rm alpine:3.17.1

# https://.docker.com/_/ruby
docker run -it --rm ruby:alpine:3.17
````

### C. CLI Utilities (Command line Interface)
Sometimes you dont have a particular utility installed on your current system, or breaking changes between versions make it handy to be able to run a specific version of a utility inside of a container without having to install anything on the host!

jq(json command line utility)

[Docerk jq](https://hub.docker.com/r/stedolan/jq)

```markdown
docker run -i stedolan/jq <sample-data/test.json '.key_1 +key_2'
```

yq(yaml command line utility)
[Docker yq](https://docker.com/r/mikefarah/yq)

```markdown
docker run -i mikefarah/yq <sample-data/test.yaml '.key_1 + .key_2'
```

#### sed
GNU **sed** behaves differently from the default MacOS version for certain edge cases.

```
docker run -i --rm busybox:1.36.0 sed 's/file.!/g' <sample-data/test.txt
```

#### base64
GNU **base64** behaves differently from the default MACOS version for certain edge cases.

```markdown
# pipe input from previous command
echo "This string is just long enough to trigger a line break in GNU base64." | docker run -i --rm busybox:1.36.0
```
#### Amazon Web Services CLI

[Docerk Amazon aws-cli](https://hub.docker.com/r/amazon/aws-cli)

```markdown
# Bind mount the credentials into the container
docker run --rm -v ~/.aws:/root.aws amazon/aws-cli:2.9.18 s3 ls
```

#### Google Cloud Platform CLI

```markdown
# Bind mount the credentials into the container
docker run --rm -v ~/config/gcloud grc.io/google.com/cloudsdktool/google-cloud-cli:415.0.0 gsutil ls
# Whi is the container image so big 🤨?! 2.8GB
```

### D. Improving the Ergonomics
if you plan to use one of these inside of a container freaquently, it can be useful to use a shell function or alias to make the ergonomics feel like the program is installed on the host.  Here are examples of this for yq:

```markdown
# Shell function
yq-shell-function() {
  docker run -rm -i -v ${PWD}:/workdir mikefarah/yg "$@"
}
yq-shell-function <sample-data/test.yaml ".key_1 + .key_2"

---

# Alias
alias 'yq-alias=docker run --rm -i -v ${PWD}:/workdor mikefarah/yq'
yq-alias <sample-data/test.yaml '.key_1 + .key_2'
```

### Bounous Material - Jessie 's talks:
**Jess Frazelle** was an early engineer at Docker (amoungst other things), where she made many contributions to the container runtime.
She also gave fun talks about doing intertesting things inside of containers.  These Two from 2015 are definitely worth a watch:

- **[Willam Wonka of Containers]()**
- **[Container Hacks and Fun Images]()**

## 05. Demo Web Application

  - One dose not simply learn docker

  

## 06. Building Container Images
   ### 01. Dockerfiles Basics
   ### 02. Dockerfile Optimization
   ### 03. Build + multi-architecture
## 07. Container Resitries
## 08. Running Containers
## 09. Container Security
## 10. Interacting with Docker Objects
## 11. Development Workflow
## 12. Deploying Containers
