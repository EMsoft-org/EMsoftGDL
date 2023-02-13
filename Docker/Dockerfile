FROM debian

RUN DEBIAN_FRONTEND=noninteractive apt-get update && apt-get -yq upgrade

# get rid of any pre-existing GDL installations
RUN apt remove gnudatalanguage

# install a bunch of packages needed to compile plplot and GDL
RUN \
        apt-get install -qy build-essential git curl x11-apps xauth cmake libgsl0-dev libplplot-dev libncurses-dev zlib1g-dev libpng-dev xorg-dev libreadline-dev libnetcdf-dev libfftw3-dev libeigen3-dev libhdf4-alt-dev libhdf5-dev pslib-dev libgraphicsmagick++-dev libudunits2-dev plplot-driver-xwin libproj-dev libwxgtk3.0-gtk3-dev libgeotiff-dev libeccodes-dev libglpk-dev

# compile and install plplot
RUN \
        mkdir PLPLOT && \
        cd PLPLOT && \
        curl -o plplot-5.15.0.tar.gz "https://newcontinuum.dl.sourceforge.net/project/plplot/plplot/5.15.0%20Source/plplot-5.15.0.tar.gz" && \
        tar -xf plplot-5.15.0.tar.gz && \ 
        mkdir build && \
        cd build && \
        cmake -DCMAKE_INSTALL_PREFIX=/usr/local \
        -DENABLE_DYNDRIVERS=ON \
        -DDEFAULT_NO_CAIRO_DEVICES=ON -DDEFAULT_NO_QT_DEVICES=ON \
        -DDEFAULT_NO_BINDINGS=ON -DENABLE_cxx=ON -DENABLE_wxwidgets=ON \
        -DOLD_WXWIDGETS=ON \
        -DPLD_pdf=OFF -DPLD_psttf=OFF ../plplot-5.15.0 && \
        make install

# compile and install GDL
RUN \ 
        git clone https://github.com/gnudatalanguage/gdl && \
        cd gdl && \
        git submodule update --init && \
        mkdir build && \
        cd build && \
        cmake .. -DGRIB=off -DPYTHON=off && \
        make -j && \
        make install

# RUN DEBIAN_FRONTEND=noninteractive apt-get install -yq gnudatalanguage 

# install a new user
ARG user=gdluser
ARG uid=501
ARG gid=3000

#Add new user with our credentials
ENV USERNAME ${user}
RUN useradd -m $USERNAME && \
        echo "$USERNAME:$USERNAME" | chpasswd && \
        usermod --shell /bin/bash $USERNAME && \
        usermod  --uid ${uid} $USERNAME && \
        groupmod --gid ${gid} $USERNAME

USER ${user}

WORKDIR /home/${user}

# create a couple of folders
RUN \ 
        mkdir /home/${user}/data && \
        mkdir /home/${user}/pro && \
        mkdir /home/${user}/Resources && \
        mkdir -p /home/${user}/.idl/emsoft/vmapps

ENV SHELL /bin/bash

