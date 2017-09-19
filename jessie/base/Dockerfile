FROM debian:jessie

MAINTAINER Geraldus <heraldhoi@gmail.com>

ENV PGVER 9.6
# fish shell requires TERM to be set
ENV TERM xterm

# Preparations for fish 2.0 installation
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
            wget \
            apt-transport-https \
 && rm -rf /var/lib/apt/lists/*

RUN wget -nv --no-check-certificate https://download.opensuse.org/repositories/shells:fish:release:2/Debian_8.0/Release.key -O Release.key \
 && apt-key add - < Release.key
RUN echo 'deb http://download.opensuse.org/repositories/shells:/fish:/release:/2/Debian_8.0/ /' \
          > /etc/apt/sources.list.d/fish.list

# Basic tools and GHC deps
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
            locales \
            git \
            bzip2 \
            ca-certificates \
            libc6-dev \
            libgmp-dev \
            libgmp10 \
            make \
            patch \
            zlib1g-dev \
            autoconf \
            automake \
            libtool \
            ncurses-dev \
            g++ \
            python \
            fish \
 && rm -rf /var/lib/apt/lists/*

# Locale setup
RUN mkdir -p ~/.config/fish \
 && echo "en_US.UTF-8 UTF-8" > /etc/locale.gen \
 && locale-gen \
 && update-locale LANG=en_US.UTF-8 \
 && echo ": ${LANG=en_US.utf8}; export LANG" >> /etc/profile \
 && echo "set -xg LANG en_US.utf8" >> ~/.config/fish/config.fish

RUN mkdir /root/tmp \
 && rm -fr /tmp/*

# PostgreSql

RUN echo 'deb http://apt.postgresql.org/pub/repos/apt/ jessie-pgdg main' > /etc/apt/sources.list.d/pgdg.list \
 && wget --quiet https://www.postgresql.org/media/keys/ACCC4CF8.asc \
 && apt-key add ACCC4CF8.asc \
 && apt-get update \
 && apt-get install -y --no-install-recommends \
            postgresql-$PGVER \
            libpq-dev \
 && rm -rf /var/lib/apt/lists/*

# Stack support

RUN useradd -m -U -s /bin/bash stack -p "stack" \
 && passwd -d stack \
 && su stack -c "mkdir /home/stack/.stack"

# Stack installation

RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 575159689BEFB442 \
 && echo 'deb http://download.fpcomplete.com/debian jessie main' | tee /etc/apt/sources.list.d/fpco.list \
 && apt-get update \
 && apt-get install -y --no-install-recommends stack \
 && rm -rf /var/lib/apt/lists/*

USER stack

RUN mkdir -p ~/.config/fish \
 && touch ~/.config/fish/config.fish \
 && echo 'set -gx PATH ~/.local/bin $PATH' > ~/.config/fish/config.fish

RUN stack upgrade \
 && stack setup

CMD ["fish"]
# CMD ["bash"]
