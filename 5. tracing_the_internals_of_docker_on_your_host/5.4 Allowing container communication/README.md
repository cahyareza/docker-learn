# PROBLEM
You want to allow communication between containers for internal purposes.

# SOLUTION
Employ user-defined networks to enable containers to communicate with each other.

# Step
First you need to create network
```terminal
$ docker network create my_network
```

This command creates a new virtual network living on your machine that you can use
to manage container communication. By default, all containers that you connect to
this network will be able to see each other by their names.

Next, assuming  that you have container blog1 and blog2 from previous section. Then you can connect network to these container
```terminal
$ docker network connect my_netwok blog1
```

Finally, you can start up new container using explisit network. And you can see you can retrieve first 5 line of blog container
```terminal
$ docker run -it --network my_network ubuntu:16.04 bash

# then
root@06d6282d32a5:/# curl -sSL blog1 | head -n5
```

