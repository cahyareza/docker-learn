# PROBLEM
You want to find out a container’s IP address.

# SOLUTION
Use the docker inspect command.

# STEP

The docker inspect command gives you access to Docker’s internal metadata in
JSON format, including the IP address. This command produces a lot of output, so
only a brief snippet of an image’s metadata is shown here.

```
docker inspect ubuntu | head
```

```
docker inspect --format '{{.NetworkSettings.IPAddress}}' 0808ef13d450
```

# DISCUSSION

Inspecting containers and the method of jumping into containers in technique 47 are
likely the two most important tools in your inventory for debugging why containers
aren’t working. Inspect shines most when you believe you’ve started a container configured in a particular way but it behaves unexpectedly—your first step should be to
inspect the container to verify that Docker agrees with your expectation of the port
and volume mappings of the container, among other things.