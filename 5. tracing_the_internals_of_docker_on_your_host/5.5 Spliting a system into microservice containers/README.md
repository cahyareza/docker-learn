# PROBLEM
You want to break your application up into distinct and more manageable services.

# SOLUTION
Create a container for each discrete service process.

# STEP

Setting up a simple PostgreSQL, NodeJS, and Nginx application
```
FROM ubuntu:14.04
RUN apt-get update && apt-get install postgresql nodejs npm nginx
WORKDIR /opt
COPY . /opt
RUN service postgresql start && \
    cat db/schema.sql | psql && \
    service postgresql stop
RUN cd app && npm install
RUN cp conf/mysite /etc/nginx/sites-available/ && \
    cd /etc/nginx/sites-enabled && \
    ln -s ../sites-available/mysite
```

there’s a problem if you want to quickly rebuild your container—any
change to any file under your repository will rebuild everything starting from the {*}
onwards, because the cache can’t be reused. If you have some slow steps (database creation or npm install), 
you could be waiting for a while for the container to rebuild.

The solution to this is to split up the COPY . /opt/ instruction into the individual
aspects of the application (database, app, and web setup).

```
FROM ubuntu:14.04
RUN apt-get update && apt-get install postgresql nodejs npm nginx
WORKDIR /opt
COPY db /opt/db                                                     -+
RUN service postgresql start && \                                    |- db setup
    cat db/schema.sql | psql && \                                    |
    service postgresql stop                                         -+
COPY app /opt/app                                                   -+
RUN cd app && npm install                                            |- app setup
RUN cd app && ./minify_static.sh                                    -+
COPY conf /opt/conf                                                 -+
RUN cp conf/mysite /etc/nginx/sites-available/ && \                  +
    cd /etc/nginx/sites-enabled && \                                 |- web setup
    ln -s ../sites-available/mysite                                 -+
```

