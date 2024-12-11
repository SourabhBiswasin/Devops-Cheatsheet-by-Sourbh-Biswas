Docker command cheat sheet links

https://quickref.me/docker.html

https://www.geeksforgeeks.org/docker-cheat-sheet/?ref=lbp

https://dockerlabs.collabnix.com/docker/cheatsheet/

# Docker volume mapping
 Docker volume mapping is used to connect a local or other container volume to a desired container volume. And during this process the data in the desired container will create duplicate data into mapped volume as well. This data duplication process is bidirection means if we create any data in mappped volume, it will also be duplicate to desired/mapped running container volume.


`docker run -d --name=<give_container_name> -v D:/<volume location of local host or of another container>:/<volume location_of_container_or_foldername_of_desired_container> <image_name> bash`

```bash
docker run -d --name=<give_container_name> -v D:/<volume location of local host or of another container>:/<volume location_of_container_or_foldername_of_desired_container> <image_name> bash
```