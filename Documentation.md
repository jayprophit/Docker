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


## Demo Application

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

## Evolution of Virtualization
### Bare Metal

||Host (physical) Machine|||
|----------|----------|----------|----------|
||Application #1|Application #2||
||Binaries/ Libraries|Binaries/ Libraries||
||Operating System|Operating System||
||Physical Hardware|Physical Hardware||

<table>
  <tr>
    <th colspan="2">Main Title Spanning Two Columns</th>
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

## 2. Technology Overview
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
