# Dockers for Trafodion/EsgynDB

This project contains dockers for building or running
Trafodion/EsgynDB.

The docker assumes the esgyn tools directory in
/data/share/esgyn/tools-el6 for CentOS 6 and
/data/share/esgyn/tools-el7 for CentOS 7. And the path for software is
/data/share/esgyn/software.

You can setup these directories in a shared directory like
/data/share, and then mount the volumn to the docker.

You can change these paths in file centos-esgyn/bashrc-el[67] before
building the docker images.

You may also need to change the file gitconfig to your user name and
email address.

## Build

```shell
$ make all
```

This will build docker images for both CentOS 6 and CentOS 7. The name
of the images are centos-esgyn:6 and centos-esgyn:7 respectively.

## Run

```shell
$ docker run -it  \ # run in interactive mode
  --name centos-esgyn7 \ # specify the docker name
  --hostname centos-esgyn7 \ # docker hostname
  --privileged \ # enable ulimit in docker
  -v /data:/data \ # mount volumn
  centos-esgyn:7 # docker image to run
```

## Running CentOS 6 Problem

Running CentOS 6 container may fail silently, this is due to change of
vsyscall in new Linux kernels. You may need to restart the host with
the following kernel parameter.

    vsyscall=emulate
