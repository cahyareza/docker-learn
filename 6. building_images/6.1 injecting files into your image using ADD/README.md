The ADD instruction in a Dockerfile serves to copy files and directories into the Docker image during the build process. It offers similar functionality to the COPY instruction but extends it with additional capabilities.
ADD can handle three types of sources:
- Local files and directories: Copies files and directories from the host machine into the Docker image.
- Tar archives: If the source is a local tar archive in a recognized compression format (gzip, bzip2, or xz), ADD automatically extracts its contents into the destination directory within the image. 
- Remote URLs: Downloads files from a specified URL and adds them to the image.

# PROBLEM
You want to download and unpack a tarball into your image in a concise way.

# SOLUTION
Tar and compress your files, and use the ADD directive in your Dockerfile.

# STEP
1. Create a fresh environment for this Docker build with mkdir add_example && cd
add_example. Then retrieve a tarball and give it a name you can reference later.


Downloading a TAR file
```
$ curl \
https://www.flamingspork.com/projects/libeatmydata/
➥ libeatmydata-105.tar.gz > my.tar.gz
```

Adding a TAR file to an image
``` 
FROM debian
RUN mkdir -p /opt/libeatmydata
ADD my.tar.gz /opt/libeatmydata/
RUN ls -lRt /opt/libeatmydata
```

# Discussion

People often ask about adding compressed files without extracting them. For this
you should use the COPY command, which looks exactly like the ADD command but
doesn’t unpack any files and won’t download over the internet.
