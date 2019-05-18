FROM amazonlinux

ARG wipdir=/opt/wip
RUN mkdir -p $wipdir

RUN yum groupinstall -y "Development Tools"

RUN yum install -y wget git patch gawk \
                   openssl-devel gmp gmp-devel \mpfr mpfr-devel libmpc libmpc-devel

# Build musl toolchain
RUN cd $wipdir && \
    git clone https://github.com/richfelker/musl-cross-make.git && \
    cd musl-cross-make && \
    cp -v config.mak.dist config.mak

RUN echo "TARGET=x86_64-linux-musl" >> $wipdir/musl-cross-make/config.mak
RUN echo "OUTPUT=/usr/local" >> $wipdir/musl-cross-make/config.mak

RUN cd $wipdir/musl-cross-make && \
    make && \
    make install

ENV CC=/usr/local/bin/x86_64-linux-musl-gcc
ENV AR=/usr/local/bin/x86_64-linux-musl-gcc-ar
ENV CXX=/usr/local/bin/x86_64-linux-musl-g++

RUN bash -c "cd /usr/bin && curl -fsSLo boot https://github.com/boot-clj/boot-bin/releases/download/latest/boot.sh && chmod 755 boot"

RUN curl --silent --location https://dl.yarnpkg.com/rpm/yarn.repo | tee /etc/yum.repos.d/yarn.repo
RUN curl --silent --location https://rpm.nodesource.com/setup_10.x | bash -

RUN yum install -y java-1.8.0-openjdk-1.8.0.171 yarn

VOLUME [ "/root/.m2", "/.boot/cache" ]

ENV BOOT_HOME /.boot
ENV BOOT_AS_ROOT yes
ENV BOOT_LOCAL_REPO /root/.m2
ENV BOOT_JVM_OPTIONS -Xmx2g
ENV BOOT_VERSION=2.8.3
ENV BUILD_STATIC_BINARY=true

WORKDIR /lumo

ENTRYPOINT bash -c "boot release"
