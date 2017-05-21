FROM debian:8.8
LABEL maintainer "Matthew Thiel <mthiel@me.com>"

RUN adduser --system eggdrop

COPY megahal.mod ./megahal.mod

RUN echo deb http://ftp.debian.org/debian jessie-backports main >> /etc/apt/sources.list \
  && apt-get update \
  && apt-get -y -t jessie-backports install gosu \
  && apt-get -y install tcl tcl-dev wget make tar build-essential libpcre3 libpcre3-dev \
  && ldconfig

RUN wget ftp://ftp.eggheads.org/pub/eggdrop/source/1.6/eggdrop1.6.21.tar.gz \
  && tar -zxvf eggdrop1.6.21.tar.gz \
  && mv megahal.mod eggdrop1.6.21/src/mod \
  && rm eggdrop1.6.21.tar.gz

RUN ( cd eggdrop1.6.21 \
    && CFLAGS="-std=gnu89" ./configure --with-tclinc=/usr/include/tcl/tcl.h --with-tcllib=/usr/lib/x86_64-linux-gnu/libtcl8.6.so \
    && make config \
    && make \
    && make install DEST=/home/eggdrop/eggdrop )

RUN rm -rf eggdrop1.6.21 \
  && mkdir /home/eggdrop/eggdrop/data \
  && chown -R eggdrop /home/eggdrop/eggdrop \
  && apt-get -y remove tcl-dev wget make build-essential libpcre3-dev

ENV NICK=""
ENV SERVER=""
ENV LISTEN="3333"
ENV OWNER=""
ENV USERFILE="eggdrop.user"
ENV CHANFILE="eggdrop.chan"

WORKDIR /home/eggdrop/eggdrop
EXPOSE 3333
COPY entrypoint.sh /home/eggdrop/eggdrop
COPY docker.tcl /home/eggdrop/eggdrop/scripts/

ENTRYPOINT ["/home/eggdrop/eggdrop/entrypoint.sh"]
CMD ["eggdrop.conf"]
