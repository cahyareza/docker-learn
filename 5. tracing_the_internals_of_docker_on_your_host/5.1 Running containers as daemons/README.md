# Problem
You want to run a Docker container in the background as a service.

# Solution
Use the -d flag to the docker run command, and use related container-management
flags to define the service characteristics.

# Step
```terminal
$ docker run -d -i -p 1234:1234 --name daemon ubuntu:14.04 nc -l 1234
```

You can now connect to it, open new terminal
```terminal
nc -v localhost 1234
```
will show 
>> Connection to localhost port 1234 [tcp/search-agent] succeeded!

