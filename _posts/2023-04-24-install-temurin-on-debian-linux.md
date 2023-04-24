---
title: Test Post
categories: [Linux, Debian, Temurin, JDK]
tags: [Linux, Debian, JDK, Adoptium, Java 17, Java 19]
published: true
---

# How to install Temurin on Debian Linux

Eclipse Temurin DEB packages are now available for installing on your Debian Linux distribution.

## Prerequisites 
```bash
apt-get install -y wget apt-transport-https gnupg
```

## Add Keyring
```bash
wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | apt-key add -
```
```bash
echo "deb https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | tee /etc/apt/sources.list.d/adoptium.list
```
```bash
apt-get update
```

## Install
```bash
apt-get install temurin-<VERSION>-jdk
```

Thats all it takes!