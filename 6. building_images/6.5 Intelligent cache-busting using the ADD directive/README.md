# PROBLEM
You want to bust the cache when a remote resource has changed.

# SOLUTION
Use the Dockerfile ADD directive to only bust the cache when the response from a
URL changes.

You’re already familiar with ADD, as it’s a basic Dockerfile directive. Normally it’s
used to add a file to the resulting image, but there are two useful features of ADD that
you can use to your advantage in this context: it caches the contents of the file it refers
to, and it can take a network resource as an argument. This means that you can bust
the cache whenever the output of a web request changes.

How can you take advantage of this when cloning a repository? Well, that depends
on the nature of the resource you’re referencing over the network. Many resources
will have a page that changes when the repository itself changes, but these will vary
from resource type to resource type. Here we’ll focus on GitHub repos, because that’s
a common use case.

The GitHub API provides a useful resource that can help here. It has URLs for
each repository that return JSON for the most recent commits. When a new commit is
made, the content of the response changes

```
FROM ubuntu:16.04
ADD https://api.github.com/repos/nodejs/node/commits
➥ /dev/null
RUN git clone https://github.com/nodejs/node
...
```

The result of the preceding listing is that the cache is busted only when a commit has
been made to the repo since the last build. No human intervention is required, and
no manual checking.

# Discussion
This technique demonstrated a valuable automated technique for ensuring builds
only take place when necessary.

It also demonstrated some of the details of how the ADD command works. You saw
that the “file” could be a network resource, and that if the contents of the file (or network resource) change from a previous build, a cache bust takes place.

In addition, you also saw that network resources have related resources that can
indicate whether the resource you’re referencing has changed. Although you could,
for example, reference the main GitHub page to see if there are any changes there,
it’s likely that the page changes more frequently than the last commit (such as if the
time of the web response is buried in the page source, or if there’s a unique reference
string in each response).

In the case of GitHub, you can reference the API, as you saw. Other services, such
as BitBucket, offer similar resources. The Kubernetes project, for example, offers this
URL to indicate which release is stable: https://storage.googleapis.com/kubernetesrelease/release/stable.txt. If you were building a Kubernetes-based project, you might
put an ADD line in your Dockerfile to bust the cache whenever this response changes.

