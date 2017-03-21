# Jupyter Notebook with jupyter-scala
# https://github.com/alexarchambault/jupyter-scala

FROM java:8

#MAINTAINER Yeongho Kim <yeonghoey@gmail.com>
MAINTAINER Orjan Lundberg <orjan.lundberg@gmail.com>

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

RUN apt-get update --fix-missing && apt-get install -y wget bzip2 ca-certificates \
    libglib2.0-0 libxext6 libsm6 libxrender1 \
    git mercurial subversion

RUN echo 'export PATH=/opt/conda/bin:$PATH' > /etc/profile.d/conda.sh && \
    wget --quiet https://repo.continuum.io/archive/Anaconda2-4.3.1-Linux-x86_64.sh -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /opt/conda && \
    rm ~/anaconda.sh

RUN apt-get install -y curl grep sed dpkg && \
    TINI_VERSION=`curl https://github.com/krallin/tini/releases/latest | grep -o "/v.*\"" | sed 's:^..\(.*\).$:\1:'` && \
    curl -L "https://github.com/krallin/tini/releases/download/v${TINI_VERSION}/tini_${TINI_VERSION}.deb" > tini.deb && \
    dpkg -i tini.deb && \
    rm tini.deb && \
    apt-get clean

ENV PATH /opt/conda/bin:$PATH

RUN curl -L -o coursier https://git.io/vgvpD && chmod +x coursier 

ENV VERSION=0.4.0 AMMONIUM_VERSION=0.8.2-2 SCALA_VERSION=2.11.8 

RUN ./coursier launch \
  -r sonatype:releases -r sonatype:snapshots \
  -i ammonite \
  -I ammonite:org.jupyter-scala:ammonite-runtime_$SCALA_VERSION:$AMMONIUM_VERSION \
  -I ammonite:org.jupyter-scala:scala-api_$SCALA_VERSION:$VERSION \
  org.jupyter-scala:scala-cli_$SCALA_VERSION:$VERSION \
  -- \
    --id scala \
    --name "Scala" \
    "$@"

ENV NBCONFIG /root/.jupyter/nbconfig
RUN mkdir -p $NBCONFIG \
 && { echo '{"CodeCell":{"cm_config":'; \
      echo '{"indentUnit":2,"smartIndent":false}'; \
      echo '}}'; \
    } > /$NBCONFIG/notebook.json

COPY bootstrap.py /bootstrap.py
RUN { echo '#!/bin/bash'; \
      echo 'set -e'; \
      echo 'python /bootstrap.py'; \
      echo 'jupyter notebook --no-browser'; \
    } > /entrypoint.sh \
 && chmod +x /entrypoint.sh

VOLUME /notebooks
WORKDIR /notebooks
EXPOSE 8888

CMD ["/entrypoint.sh"]
