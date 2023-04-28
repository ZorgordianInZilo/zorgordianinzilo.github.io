---
layout: post
title: Executing Docker command without Sudo
categories: [Linux, Docker]
tags: [Linux, Docker, images, containers, volumes HowTo, Guide]
#published: false
---

If you want to avoid typing sudo whenever you run the docker command, add your username to the docker group:

```bash
sudo usermod -aG docker ${USER}
```

To apply the new group membership, log out of the server and back in, or type the following:

```bash
su - ${USER}
```

You will be prompted to enter your user’s password to continue.

***

If you need to add a user to the docker group that you’re not logged in as, declare that username explicitly using:
```bash
sudo usermod -aG docker <username>
```