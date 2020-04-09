---
title: How to use Docker
updated: 2020-04-09 10:30
---

* TOC
{:toc}

# Terminologies of Docker


### Docker File

* Dockerfile is **a script of instructions** to build a customized image layer by layer.

* To build a docker image from a docker file, run
```
docker image build -t <image_name> <path_to_dockerfile>
```

* If you receive an already built docker image (a tar archive) from others, you can load it into your docker pool by running
```
sudo docker load --input <path_to_docker_image_tar>
```

### Docker Image

* A docker image is essentially **a snapshot file of a virtual environment**. It is dead and immutable, and can be shared or transferred.

* To list all the docker images in your docker pool, run
```
docker image ls or docker images
```

* To remove a docker image, run
```
docker image rm <image_name or id>
```

### Docker Container

* A container amounts to **a running image**. When one creates an runnable container from an image, it will automatically recover the virtual environment written in the image file and bring the applications of the environment into life. Every container **has a own copy of the image layers** defined by a docker image base and additionally add a writable container layer on top of them so that we can edit it by installing any other applications. Therefore, a container is also very large. You can create as many containers from a docker image as you want.

* To create a container from an image base, run
```
docker create -v <path_in_local>:<path_in_container> --name <container_name>  <image_name:image_tag or image_id>
# option "-v" bind mounts a volume from local machine to a location inside the container.
```

* To list all the containers in your docker pool, run
```
docker container ls -a or docker ps -a
# option "-a" lists the containers even they are not running.
# "docker container ls" == "docker ps"
``` 

* To start a container, run
```
docker (container) start <container_id>
# After a container is started, it will run as a daemon in the background.
# "container" in the parentheses is optional.
```

* To run a command in a running container, run
```
docker exec -it <container_name or id> bash
# "bash" will create a bash session
```

* To create and run a container from an image base at the same time, run
```
docker run -v <path_in_local>:<path_in_container> --name <container_name>  <image_name:image_tag or image_id>
```

* To stop a running container, run
```
docker stop <container_name or id>
```

* To rename a container, run
```
docker rename <container_name or id> <new_name>
```

* To remove a container from your docker pool, run
```
docker rm <container_name or id>
```