Docker stores all the data relating to your containers and images under a folder.
Because it can store a potentially large number of different images, this folder can get
big fast!

# PROBLEM
You want to move where Docker stores its data.

# SOLUTION
If running the actual dockerd (daemon):
You can configure the Docker daemon with a daemon.json file or command-line flags.

To change the Docker data root directory (/var/lib/docker by default) to, say, /custom/docker, you do this:

Create or edit the file:
```
sudo mkdir -p /etc/docker
sudo nano /etc/docker/daemon.json
```

Add this content:
```
{
  "data-root": "/custom/docker"
}
```

Then restart the daemon:
```
brew services restart docker

```