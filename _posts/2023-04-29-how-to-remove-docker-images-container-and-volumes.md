---
layout: post
title: How to remove Docker Images, Containers and Volumes
categories: [Linux, Docker]
tags: [Linux, Docker, images, containers, volumes HowTo, Guide]
#published: false
---

# Introduction
By using Docker, you can package your applications and services in containers, enabling you to execute them in any environment. Nevertheless, over time, you may end up with a surplus of unused images, containers, and data volumes that take up valuable disk space and make the output messy.

Fortunately, Docker offers a range of command-line tools to help you tidy up your system. This concise guide presents a cheat-sheet-style reference to useful commands that will assist you in freeing up disk space, organizing your system, and removing unneeded Docker images, containers, and volumes.

# Purge All Unused
Docker offers a convenient way to eliminate any dangling resources - such as images, containers, volumes, and networks - using a single command:

```bash
docker system prune
```


To go a step further and delete all unused images (not just dangling ones) and stopped containers, you can include the -a flag:

``` bash
docker system prune -a
```

# Removing Docker Images
## Remove one or more specific images

To find the ID of the images you wish to delete, utilize the docker images command with the -a flag. This will display all images, including intermediate image layers. Once you have identified the images you want to remove, you can pass their ID or tag to docker rmi.

### List:
```bash
docker images -a
```

### Remove:
```bash
docker rmi <IMAGE>
```

## Remove Dangling Images
Docker images are composed of multiple layers, and some of these layers may become unused over time. These unused layers, known as dangling images, take up valuable disk space and serve no purpose. To locate dangling images, use the docker images command with the filter flag -f and set its value to dangling=true. Once you have identified the images you want to remove, you can utilize the docker image prune command to delete them.

> Note: If you create an image without a tag, it will be considered a dangling image because it is not associated with any tagged images. To avoid this, ensure that you provide a tag when building an image. You can also assign a tag to an image retroactively using the docker tag command.

### List:
```bash
docker images -f dangling=true
```
### Remove:
```bash
docker image prune
```

## Removing images according to a pattern
To locate all images that match a particular pattern, you can use a combination of docker images and grep. After verifying the list of images, you can remove them by using awk to extract their IDs and passing them to docker rmi. It's important to note that these utilities are not provided by Docker and may not be available on every system.
### List:
```bash
docker images -a |  grep "pattern"
```
### Remove:
```bash
docker images -a | grep "pattern" | awk '{print $3}' | xargs docker rmi
```

## Removing all images
To display all Docker images present on a system, append -a to the docker images command. If you are certain that you want to delete all of them, you can add the -q flag to pass the image ID to docker rmi.
### List:
```bash
docker images -a
```
### Remove:
```bash
docker rmi $(docker images -a -q)
```

# Removing Containers
## Remove one or more specific containers

To identify the name or ID of the containers you wish to delete, utilize the docker ps command with the -a flag:

### List:
```bash
docker ps -a
```

### Remove:
```bash
docker rm <ID_or_Name> <ID_or_Name>
```

## Remove a container upon exiting
If you are aware that you won't require a container after it has served its purpose, you can execute the docker run --rm command while creating it. This will ensure that the container is deleted automatically once it exits.

### Run and Remove:
```bash
docker run --rm <image_name>
```

## Remove all exited containers
To locate containers, utilize the docker ps -a command and apply filters based on their status, which can be created, restarting, running, paused, or exited. You can narrow down the list of exited containers by applying the -f flag and filtering based on their status. After verifying that you wish to remove those containers, use -q to pass their IDs to the docker rm command.

### List:
```bash
docker ps -a -f status=exited
```
### Remove:
```bash
docker rm $(docker ps -a -f status=exited -q)
```

## Remove containers using more than one filter
To apply multiple filters to Docker, repeat the filter flag with a new value. This will result in a list of containers that meet either condition. For instance, if you aim to remove all containers labeled as either created (a state that may occur when running a container with an invalid command) or exited, you can employ two filters.
### List:
```bash
docker ps -a -f status=exited -f status=created
```
### Remove:
```bash
docker rm $(docker ps -a -f status=exited -f status=created -q)
```

## Removing containers according to a pattern
To locate all the containers that match a particular pattern, you can use a combination of docker ps and grep. Once you have verified that the list contains the containers you want to delete, you can use awk and xargs to provide the ID to docker rm. It is important to note that these utilities are not provided by Docker and may not be available on every system.

### List:
```bash
docker ps -a |  grep "pattern”
```

### Remove:
```bash
docker ps -a | grep "pattern" | awk '{print $1}' | xargs docker rm
```

## Stop and remove all containers
You can view the containers on your system using the docker ps command. By appending the -a flag, you can view all containers. After you have confirmed that you want to remove them, you can include the -q flag to provide IDs to the docker stop and docker rm commands:

### List:
```bash
docker ps -a
```
### Remove:
```bash
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
```

# Removing Volumes
## Remove one or more specific volumes
To identify the name or names of the volume that you want to delete, you can use the docker volume ls command. After locating the desired volume(s), you can remove one or more of them using the docker volume rm command:

### List:
```bash
docker volume ls
```
### Remove:
```bash
docker volume rm <volume_name> <volume_name>
```

## Remove dangling volumes
Volumes are designed to exist independently of containers, so removing a container does not automatically remove the associated volume. When a volume is no longer connected to any containers, it is considered a dangling volume. You can use the docker volume ls command with a filter to limit the results to dangling volumes and confirm that you want to remove them. Once you're satisfied with the list, you can remove all dangling volumes using the docker volume prune command:

### List:
```bash
docker volume ls -f dangling=true
```
### Remove:
```bash
docker volume prune
```

## Remove a container and its volume
To delete an unnamed volume that was created with a container, you can use the -v flag with the docker rm command. However, if the volume was named, it will remain on the system even after the container is removed. When the container is successfully removed with the -v flag, its ID is displayed, but no indication is given about the removal of the volume.

### Remove:
```bash
docker rm -v container_name
```

# Conclusion

This guide provides an overview of some of the basic commands used to remove images, containers, and volumes in Docker. However, there are many other combinations and flags available for each of these commands. For more detailed information, refer to the Docker documentation for docker system prune, docker rmi, docker rm, and docker volume rm. If you have any suggestions for common cleanup tasks that should be included in this guide, feel free to ask or provide your feedback in the comments.