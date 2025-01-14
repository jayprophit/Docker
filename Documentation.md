# Complete Docker Course - From Beginner to Pro(Learn Containers)

## Prerequisites
- Familiarity with web applications (the course examples are in JavaScript + Golang)
- Basic shell commands (enough to build/ run an application in a language of your own choosing on Linux

## Course Overview
1. History and Motivation
2. Technology Overview
   1. Containers
   2. Docker
3. Installation/ Sewt up & hello World
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


## 1. History and Motivation
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

Note: There are a new class of micro-VMs designd to start in seconds! (research "Firecracker VM" for more info!)


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
Note: There is much more nuance to "performance" than this chart can capture. A VM or container dosen't inherently sacrice much performance relative to bare metal it runs on, but being able to have more controlover things like connected storage, physical proximity of the system relative to others it communicates with, specific hardware accelerators, etc... do enable performance tuning.

## 2. Technology Overview
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




   ### 1. Containers
   ### 2. Docker
## 3. Installation/ Sewt up & hello World
## 4. Using 3rd party containers
## 5. Demo Application
## 6. Building Container Images
   ### 1. Dockerfiles Basics
   ### 2. Dockerfile Optimization
   ### 3. Build + multi-architecture
## 7. Container Resitries
## 8. Running Containers
## 9. Container Security
## 10. Interacting with Docker Objects
## 11. Development Workflow
## 12. Deploying Containers
