# Docker Tips & Tricks
Some best practices of docker and microservices

## Before start editing
1. [Here is the link to markdown cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

# Introduction
This helps in understanding some tips and best practices while building containers with Docker.

Some of the Best practices are documented in the link:

1. [Docker Best practices](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/)
2. [Why using multiple RUN in docker file does not solve long RUN lines](http://stackoverflow.com/questions/25943274/docker-run-command-when-to-group-commands-when-not-to)

# Highlights of Best practices
1. Run only one process per container
2. Minimize the number of layers
3. Sort arguments which are passed to scripts or commands.
4. [Why to run apache in foreground](https://coreos.com/os/docs/latest/getting-started-with-docker.html):Run Apache in foreground. We need to run the apache process in the foreground, since our container will stop when the process specified in the docker run command stops. We can do this with a flag -D when starting the apache2 process: ``` /usr/sbin/apache2ctl -D FOREGROUND```
