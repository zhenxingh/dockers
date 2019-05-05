ARG osver=7
FROM centos:${osver}
ARG user=${user:-esgyn}
ARG osver=7

# Install building environment dependencies
RUN yum install -y redhat-lsb
RUN yum install -y epel-release
RUN yum install -y alsa-lib-devel ant ant-nodeps apr-devel apr-util-devel \
         boost-devel cmake device-mapper-multipath dhcp doxygen flex \
         gcc-c++ gd git glibc-devel glibc-devel.i686 graphviz-perl gzip \
         java-1.7.0-openjdk-devel java-1.8.0-openjdk-devel \
         libX11-devel libXau-devel libaio-devel \
         libcurl-devel libibcm.i686 libibumad-devel libibumad-devel.i686 \
         libiodbc libiodbc-devel librdmacm-devel librdmacm-devel.i686 \
         libxml2-devel lua-devel lzo-minilzo lzo lzo-devel snappy snappy-devel\
         net-snmp-devel net-snmp-perl openldap-clients openldap-devel \
         openldap-devel.i686 openmotif openssl-devel openssl-devel.i686 \
         perl-Config-IniFiles perl-Config-Tiny \
         perl-DBD-SQLite perl-Expect perl-IO-Tty perl-Math-Calc-Units \
         perl-Params-Validate perl-Parse-RecDescent perl-TermReadKey \
         perl-Time-HiRes protobuf-compiler protobuf-devel \
         libcgroup libcgroup-tools libcgroup-devel net-tools\
         readline-devel rpm-build saslwrapper sqlite-devel libuuid-devel\
         unixODBC unixODBC-devel uuid-perl wget xerces-c-devel xinetd ncurses-devel

# Install SSHD
RUN yum install -y openssh-server

# Install sudo
RUN yum install -y sudo

# Install bower for compiling dbmgr
RUN yum install -y npm
RUN npm --registry https://registry.npm.taobao.org install -g bower

# For EsgynDB Audit feature
RUN yum install -y zeromq-devel

RUN yum install -y emacs

# Cleanup yum
RUN yum clean all

# Generate ssh host keys
RUN ssh-keygen -f /etc/ssh/ssh_host_rsa_key -N '' -t rsa
RUN ssh-keygen -f /etc/ssh/ssh_host_ecdsa_key -N '' -t ecdsa
RUN if [ x${osver} = x7 ]; then ssh-keygen -f /etc/ssh/ssh_host_ed25519_key -N '' -t ed25519; fi
RUN if [ x${osver} = x6 ]; then ssh-keygen -f /etc/ssh/ssh_host_dsa_key -N '' -t dsa; fi

# Setup ulimit for running esgyndb
COPY limits.conf /etc/security/limits.conf
RUN rm -f /etc/security/limits.d/*-nproc.conf

# Add user
RUN useradd -m -U -u 1000 ${user}
RUN echo "${user} ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/${user}
RUN chmod 0644 /etc/sudoers.d/${user}

# Setup user environment
USER ${user}

# Generate user ssh keys
RUN ssh-keygen -q -t rsa -N '' -f ~/.ssh/id_rsa
RUN cp -a ~/.ssh/id_rsa.pub ~/.ssh/authorized_keys
RUN echo "NoHostAuthenticationForLocalhost=yes" >>~/.ssh/config

# Setup the symlinks for building esgyndb
RUN ln -s /data/home/${user}/work /home/${user}/work
RUN ln -s /data/share/m2 /home/${user}/.m2
RUN mkdir /home/${user}/.cache
RUN ln -s /data/share/bower /home/${user}/.cache/bower

# Add environs for building esgyndb
COPY bashrc-el${osver} /home/${user}/.bashrc
COPY gdbinit /home/${user}/.gdbinit
COPY gitconfig /home/${user}/.gitconfig

WORKDIR /root
USER root
ENTRYPOINT /usr/sbin/sshd && /bin/bash
