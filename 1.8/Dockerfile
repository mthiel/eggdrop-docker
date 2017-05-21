FROM debian:8.8
LABEL maintainer "Matthew Thiel <mthiel@me.com>"

RUN adduser --system eggdrop

COPY megahal.mod ./megahal.mod

RUN echo deb http://ftp.debian.org/debian jessie-backports main >> /etc/apt/sources.list \
  && apt-get update \
  && apt-get -y -t jessie-backports install gosu \
  && apt-get -y install tcl tcl-dev wget make tar build-essential libpcre3 libpcre3-dev \
  && ldconfig

RUN wget ftp://ftp.eggheads.org/pub/eggdrop/source/1.8/eggdrop-1.8.1.tar.gz \
  && tar -zxvf eggdrop-1.8.1.tar.gz \
  && mv megahal.mod eggdrop-1.8.1/src/mod \
  && rm eggdrop-1.8.1.tar.gz

RUN ( cd eggdrop-1.8.1 \
    && ./configure \
    && make config \
    && make \
    && make install DEST=/home/eggdrop/eggdrop )

RUN rm -rf eggdrop-1.8.1 \
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
