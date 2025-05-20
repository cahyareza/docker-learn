# PROBLEM
You want to rebuild your Dockerfile without using the cache.

# SOLUTION
To force a rebuild without using the image cache, run your docker build with the
--no-cache flag. The following listing runs the previous build with --no-cache.

Forcing a rebuild without using the cache
```
$ docker build --no-cache .
```

# DISCUSSION
Removing caching can be a useful sanity check once you’ve got your final Dockerfile,
to make sure it works from top to bottom, particularly when you’re using internal web
resources in your company that you may have changed while iterating on the Dockerfile. This situation doesn’t occur if you’re using ADD, because Docker will download
the file every time to check if it has changed, but that behavior can be tiresome if
you’re pretty sure it’s going to stay the same and you just want to get going with writing the rest of the Dockerfile.

