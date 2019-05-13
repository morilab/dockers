#docker run -v c:/docker/modelsim-ise/prime_pro_19.1/files/:/tmp/files/ -it ubuntu:18.04 /bin/bash
#docker build -t modelsim-ise:prime_pro_19.1 .

FROM ubuntu:18.04
MAINTAINER morilab

ADD files/ModelSimProSetup-19.1.0.240-linux.run /tmp/files
ADD files/modelsim-part2-19.1.0.240-linux.qdz   /tmp/files
ENV USER genetec
ENV HOME /home/${USER}

#--------------------------------------------------------------------------------------------------------------
WORKDIR /tmp

RUN set -x && : "Ubuntuのベース環境を整える" \
 && dpkg --add-architecture i386 \
 && apt update \
 && apt upgrade -y \
 && apt-get install -y wget build-essential

RUN set -x && : "ModelSim用ライブラリ : download" \
 && wget http://security.ubuntu.com/ubuntu/pool/main/libp/libpng/libpng12-0_1.2.54-1ubuntu1.1_i386.deb  \
 && wget http://security.ubuntu.com/ubuntu/pool/main/libp/libpng/libpng12-0_1.2.54-1ubuntu1.1_amd64.deb \
 && wget http://archive.ubuntu.com/ubuntu/pool/main/f/freetype/libfreetype6_2.6.1-0.1ubuntu2_i386.deb   \
 && wget http://archive.ubuntu.com/ubuntu/pool/main/f/freetype/libfreetype6_2.6.1-0.1ubuntu2_amd64.deb

RUN set -x && : "ModelSim用ライブラリ : install(1)"   \
 && apt-get -f install -y \
 && dpkg -i libpng12-0_1.2.54-1ubuntu1.1_amd64.deb  \
 && dpkg -i libfreetype6_2.6.1-0.1ubuntu2_amd64.deb

RUN set -x && : "ModelSim用ライブラリ : install(2)"   \
 && apt-get -f install -y \
 && dpkg -i libpng12-0_1.2.54-1ubuntu1.1_i386.deb   \
 && dpkg -i libfreetype6_2.6.1-0.1ubuntu2_i386.deb

RUN set -x && : "ModelSim用のライブラリをインストールする" \
 && apt-get install -y gcc-multilib g++-multilib \
        lib32z1 lib32stdc++6 lib32gcc1 \
        expat:i386 fontconfig:i386 libfreetype6:i386 libexpat1:i386 libc6:i386 libgtk-3-0:i386  \
        libcanberra0:i386 libpng12-0:i386 libice6:i386 libsm6:i386 libncurses5:i386 zlib1g:i386 \
        libx11-6:i386 libxau6:i386 libxdmcp6:i386 libxext6:i386 libxft2:i386 libxrender1:i386   \
        libxt6:i386 libxtst6:i386


RUN set -x && : "ModelSimインストール" \
 && chmod +x files/*-linux.run \
 && chmod +r files/* \
 && ./files/*-linux.run --mode unattended --accept_eula 1 --installdir /opt \
 && cd /opt/modelsim_ase \
 && ln -s linuxaloem linux_rh60
 
RUN set -x && : "ユーザー作成" \
 && useradd -m ${USER} \
 && gpasswd -a ${USER} sudo \
 && echo "${USER}:${USER}" | chpasswd

RUN set -x && : "ユーザー環境構築" \
 && apt-get install -y git vim


#--------------------------------------------------------------------------------------------------------------
WORKDIR ${HOME}

USER ${USER}
ENV PATH $PATH:/opt/modelsim_ase/bin
CMD vsim -version
