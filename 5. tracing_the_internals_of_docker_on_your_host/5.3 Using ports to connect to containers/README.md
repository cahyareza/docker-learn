# PROBLEM
You want to make multiple Docker container services available on a port from your
host machine.
# SOLUTION
Use Docker’s -p flag to map a container’s port to your host machine.

# Step
Obtain images from external locations, you can use the docker pull command. By default, images will be downloaded
from the Docker Hub:
```
$ docker pull tutum/wordpress
```

Images will also be retrieved automatically when you try to run them if they’re not
already present on your machine.
To run the first blog, use the following command:
```
$ docker run -d -p 10001:80 --name blog1 tutum/wordpress
```
This docker run command runs the container as a daemon (-d) with the publish flag
(-p). It identifies the host port (10001) to map to the container port (80) and gives
the container a name to identify it (--name blog1 tutum/wordpress).

You can do the same for the second blog:
```
$ docker run -d -p 10002:80 --name blog2 tutum/wordpress
```
then
```
$ docker ps | grep blog
```
you’ll see the two blog containers listed, with their port mappings, looking something