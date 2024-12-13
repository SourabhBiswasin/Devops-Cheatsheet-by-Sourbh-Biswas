Docker command cheat sheet links

https://quickref.me/docker.html

https://www.geeksforgeeks.org/docker-cheat-sheet/?ref=lbp

https://dockerlabs.collabnix.com/docker/cheatsheet/

# Docker volume mapping
 Docker volume mapping is used to connect a local or other container volume to a desired container volume. And during this process the data in the desired container will create duplicate data into mapped volume as well. This data duplication process is bidirection means if we create any data in local or other container mappped volume, it will also be duplicate to desired mapped running container volume as well.

Example Code
```bash
docker run -d --name=<give_container_name> -v D:/<volume location of local host or of another container>:/<volume location_of_container_or_foldername_of_desired_container> <image_name> bash
```
-v refer to volume mapping
-d refer to run container is detch mode

Note: - Volume mapping can be done on while creating a new container only. WE can't do it on already available containers.

## Remove docker volume mapping

First stop the container and remove the volume using command

```bash
docker rm volume <volume_name>
```
or 

```bash
docker rm volume prune
```
# Docker port mapping

```bash
docker run -d -p 8009<enter_port_as_per_our_need>:80<default_port_of_container_or_image> <image_name>
```
-p refer to port mapping
-d refer to run container is detch mode

## Rename docker image

```bash
docker tag <old_image_name> <new_image_name>
```

# [Docker compose documentation](https://docs.docker.com/reference/compose-file/services/)
 - Docker compose is used to manage multiple container such as creating containers, volume mapping, port mapping and networks.
