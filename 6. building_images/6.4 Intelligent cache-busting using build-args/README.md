# PROBLEM
You want to bust the cache on demand when performing a build, without editing the
Dockerfile.

# SOLUTION
Use the ARG directive in your Dockerfile to enable surgical cache-busting.

To demonstrate this, you’re again going to use the Dockerfile at https://github
.com/docker-in-practice/todo, but make a minor change to it.

What you want to do is control the busting of the cache before the npm install.
Why would you want to do this? As you’ve learned, by default Docker will only break
the cache if the command in the Dockerfile changes. But let’s imagine that updated
npm packages are available, and you want to make sure you get them. One option is
to manually change the line (as you saw in the previous technique), but a more elegant way to achieve the same thing involves using the Docker ARGS directive and a
bash trick.

Add the ARG line to the Dockerfile as follows.
```
WORKDIR todo
ARG CACHEBUST=no
    RUN npm install
```
In this example, you use the ARG directive to set the CACHEBUST environment variable
and default it to no if it’s not set by the docker build command.

At this point you decide that you want to force the npm packages to be rebuilt. Perhaps a bug has been fixed, or you want to be sure you’re up to date. This is where the
ARG variable you added to the Dockerfile in listing 4.7 comes in. If this ARG variable is
set to a value never used before on your host, the cache will be busted from that point.
 
This is where you use the build-arg flag to docker build, along with a bash trick
to force a fresh value:

```
docker build --build-arg CACHEBUST=${RANDOM}
```

The use of the ${RANDOM} argument is worth explaining. Bash provides you with
this reserved variable name to give you an easy way of getting a value between one and
five digits long

# DISCUSSION
This technique has demonstrated a few things that will come in handy when using
Docker. You’ve learned about using the --build-args flag to pass in a value to
the Dockerfile and bust the cache on demand, creating a fresh build without changing the Dockerfile.

If you use bash, you’ve also learned about the RANDOM variable, and how it can be
useful in other contexts than just Docker builds.