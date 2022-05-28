FROM archlinux:latest
COPY pacman.conf /etc/pacman.conf
COPY sudoers /etc/sudoers
RUN export TERM=xterm && pacman -Sy && \
      pacman -S --noconfirm base-devel vim lib32-gcc-libs \
      lib32-ncurses cpio file \
      wget texinfo zip unzip git && \
      pacman -Scc --noconfirm
ARG GID=1000
ARG UID=1000
RUN groupadd --gid=$GID lotus && \
      useradd --gid=$GID --uid=$GID -G wheel -m -d /home/lotus -s /bin/bash lotus && \
      gpasswd -a lotus wheel
COPY --chown=lotus:lotus l123set.cf /home/lotus/.l123set
COPY docker/build_binutils.sh /usr/local/bin/
RUN mkdir -p /home/lotus/123elf && \
      chown -R lotus:lotus /home/lotus/123elf
WORKDIR /home/lotus/123elf

COPY --chown=lotus:lotus . ./
USER lotus
RUN unzip docker/123-unix.zip -d ./
RUN TERM=linux /usr/local/bin/build_binutils.sh && \
      ./extract.sh  &&\
      make
ENTRYPOINT ["./123"]
