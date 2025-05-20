# PROBLEM
You want to invalidate the Docker build cache from a specific point in the Dockerfile
build.

# SOLUTION
Add a benign comment after the command to invalidate the cache.

# DISCUSSION
For the initial iteration on a Dockerfile, splitting up every single command into a
separate layer is excellent for speed of iteration, because you can selectively rerun parts
of the process, as shown in the previous listing, but it’s not so great for producing a
small final image. It’s not unheard-of for builds with a reasonable amount of complexity
to approach the hard limit of 42 layers. To mitigate this, once you have a working build
you’re happy with, you should look at the steps in technique 56 for creating a
production-ready image.