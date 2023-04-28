---
layout: post
title: How to Install Docker on Ubuntu
categories: [Linux, Docker, Ubuntu]
tags: [Linux, Docker, Ubuntu, images, containers, volumes HowTo, Guide]
#published: false
---

# Introduction

Docker is a tool designed to simplify the management of application processes in containers, which allow for running applications in resource-isolated processes. While similar to virtual machines, containers are more portable, less resource-intensive, and rely heavily on the host operating system.

To learn more about the various components of a Docker container, you can refer to "The Docker Ecosystem: An Introduction to Common Components."

# Installing Docker
To obtain the most recent version of Docker, we will install it from the official Docker repository instead of using the one available in the Ubuntu repository. This involves adding the Docker package source, verifying the authenticity of the downloads by adding the Docker GPG key, and finally installing the package.

First, update your existing list of packages:
```bash
sudo apt update
```
Next, install a few prerequisite packages which let apt use packages over HTTPS:
```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```
Then add the GPG key for the official Docker repository to your system:
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
Add the Docker repository to APT sources:
```bash
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
```
Finally, install Docker:
```bash
sudo apt install docker-ce
```
Docker should now be installed, the daemon started, and the process enabled to start on boot.