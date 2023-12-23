FROM rockylinux:9

RUN dnf install -q -y \
    diffutils \
    gcc \
    gcc-c++ \
    sudo

RUN dnf install -q -y \
    openssh-clients \
    openssh-server

# Since GPDB cannot run under root, create gpadmin user
RUN groupadd -g 1100 gpadmin && useradd -u 1100 -g 1100 gpadmin && \
    echo "gpadmin ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/gpadmin && \
    groupadd supergroup && usermod -a -G supergroup gpadmin && \
    mkdir /home/gpadmin/.ssh && \
    ssh-keygen -t rsa -N "" -f /home/gpadmin/.ssh/id_rsa && \
    cp /home/gpadmin/.ssh/id_rsa.pub /home/gpadmin/.ssh/authorized_keys && \
    chmod 0600 /home/gpadmin/.ssh/authorized_keys && \
    echo "gpadmin:gpadmin" | chpasswd && \
    chown -R gpadmin:gpadmin /home/gpadmin/.ssh

RUN dnf install -q -y \
    flex \
    bison \
    perl \
    llvm-devel \
    llvm-toolset \
    clang \
    libzstd-devel \
    zlib-devel \
    apr-util-devel \
    libevent-devel

RUN dnf install -q -y --enablerepo=crb \
    libyaml-devel

RUN dnf install -q -y \
    libxml2-devel \
    libcurl-devel \
    bzip2-devel \
    libxslt-devel

RUN dnf install -q -y \
    python3-psycopg2 \
    python3-psutil

RUN echo "/usr/local/lib64" >>/etc/ld.so.conf.d/gpdb.conf && \
    ldconfig

RUN dnf install -q -y \
    python-devel

RUN dnf install -q -y \
    hostname \
    iproute \
    iputils \
    rsync

RUN echo "gpadmin soft core unlimited" >  /etc/security/limits.d/gpadmin-limits.conf && \
    echo "gpadmin soft nproc 131072"   >> /etc/security/limits.d/gpadmin-limits.conf && \
    echo "gpadmin soft nofile 65536"   >> /etc/security/limits.d/gpadmin-limits.conf

RUN ssh-keygen -A
RUN chmod 4755 /bin/ping

ADD entrypoint.sh /entrypoint.sh
RUN chmod 755 /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
