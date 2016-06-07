GitLab deployment for my home network.


Docker and Docker Compose
-------------------------

The intent behind Docker and Docker Compose on this
installation is to isolate and track all dependencies
and to allow for more aggressive (and less risky)
experimentation of different configurations.

This setup requires both system mapped volumes and a
system mapped IP address.  It is in no way attempting
to be either scalable or redundant.

If you are new to Docker Compose, you may wish to
stumble through their introductory material before
fooling around too much with this installation.

https://docs.docker.com/v1.7/compose/


gitlab
------

Hostname and ports in `docker-compose.yml` need to be
updated to reflect the local configuration.  See guides
elsewhere for instructions on how to set up a second
IP address on your own machine (try a Google search for
"second IP <your operating system>").

Note that GitLab has its own SSH server... which likely
means that you'll need to limit which IP addresses the SSH
server of your own machine is taking for itself.

After necessary modifications are made, use docker-compose
to bring up the service.

```
# docker-compose up -d gitlab
```

TODO: Move hostname and IP to init script.


gitlab-runner
-------------

All runners must be registered with GitLab to start
processing builds.  For that, you'll need the registration
token from GitLab.

http://gitlab/admin/runners

Once you have the registration token, start up the runner
and register it.

```
# docker-compose up -d gitlab-runner
# docker-compose run --rm gitlab-runner register
```


init
----

The init script should be linked into the sysvinit
directory.  The `DOCKER_COMPOSE_YML` path must be updated
to match the location that gitlab is deployed on the server.

```
# ln -s "$(pwd)"/link /etc/init.d/gitlab
```

TODO: [LSB compliance](https://wiki.debian.org/LSBInitScripts)
