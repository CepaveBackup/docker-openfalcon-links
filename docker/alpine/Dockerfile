FROM alpine:3.4

MAINTAINER minimum@cepave.com

ARG DOCKERDIR=docker/alpine
ARG PACKFILE=falcon-links.tar.gz
ARG CONFIGDIR=/config
ENV WORKDIR=/home/links

# Set timezone, bash, config dir
# Set alias in the case of user want to execute control in their terminal
RUN \
  apk add --no-cache tzdata bash \
  && cp /usr/share/zoneinfo/Asia/Taipei /etc/localtime \
  && echo "Asia/Taipei" > /etc/timezone \
  && echo "alias ps='pstree'" > ~/.bashrc \
  && mkdir -p $CONFIGDIR

# Install Open-Falcon Links Component
RUN apk add --no-cache python-dev py-virtualenv py-mysqldb
ADD $DOCKERDIR/$PACKFILE $WORKDIR
ARG CONFIGFILE=config.py
COPY src/config.py $CONFIGDIR/$CONFIGFILE
RUN \
  ln -sf $CONFIGDIR/$CONFIGFILE $WORKDIR/frame/config.py \
  && virtualenv $WORKDIR/env \
  && pip install -r $WORKDIR/pip_requirements.txt

WORKDIR $WORKDIR
COPY $DOCKERDIR/run.sh $WORKDIR/

# Port
EXPOSE 5090

# Start
ENTRYPOINT ["/bin/bash"]
CMD ["run.sh"]