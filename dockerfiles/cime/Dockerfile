FROM ubuntu:20.10 as v1

ENV USER=root
ENV CIME_MODEL=e3sm
ENV NETCDF_FORTRAN_VERSION=4.5.2
ENV PNETCDF_VERSION=1.12.1
ENV PATH=/build/local/bin:$PATH

WORKDIR /build

RUN apt update && \
      apt install -y --no-install-recommends \
        python3 \
        git \
        vim \
        build-essential \
        gcc-9 \
        g++-9 \
        gfortran-9 \
        openmpi-bin \
        libopenmpi-dev \
        libcurl4-openssl-dev \
        libnetcdf-dev \
        netcdf-bin \
        libxml2-utils \
        pylint \
        cmake \
        curl \
      pkg-config && \
      rm -rf /var/lib/apt/lists && \
      update-alternatives --install /usr/bin/python python /usr/bin/python3 10 && \
      update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 10 && \
      update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-9 10 && \
      update-alternatives --install /usr/bin/gfortran gfortran /usr/bin/gfortran-9 10 && \
      curl -L -k -o netcdf-fortran.tar.gz \
      https://github.com/Unidata/netcdf-fortran/archive/refs/tags/v${NETCDF_FORTRAN_VERSION}.tar.gz && \
      mkdir "${PWD}/netcdf-fortran" && \
      tar -xvf "${PWD}/netcdf-fortran.tar.gz" -C "${PWD}/netcdf-fortran" --strip-components=1 && \
      cd "${PWD}/netcdf-fortran" && \
      ./configure --prefix=/build/local && \
      make && \
      make install && \
      curl -L -k -o pnetcdf.tar.gz \
      https://parallel-netcdf.github.io/Release/pnetcdf-${PNETCDF_VERSION}.tar.gz && \
      mkdir "${PWD}/pnetcdf" && \
      tar -xvf "${PWD}/pnetcdf.tar.gz" -C "${PWD}/pnetcdf" --strip-components=1 && \
      cd "${PWD}/pnetcdf" && \
      ./configure --prefix=/build/local --enable-shared --disable-cxx && \
      make && \
      make install && \
      cd "/build/local/include" && \
      ln -sf /usr/include/*netcdf* . && \
      cd "/build/local/lib" && \
      ln -sf `nc-config --libdir`/lib* .

RUN mkdir /root/.cime

COPY config_machines.xml /root/.cime/
COPY config_compilers.xml /root/.cime/

FROM ubuntu:20.10 as v2

WORKDIR /build

RUN apt update && \
      apt install -y --no-install-recommends \
      python3 \
      build-essential \
      gcc-9 \
      g++-9 \
      gfortran-9 \
      openmpi-bin \
      libopenmpi-dev \
      libcurl4-openssl-dev \
      libnetcdf-mpi-15 \
      libnetcdf-mpi-dev \
      libnetcdf-pnetcdf-15 \
      libnetcdf-pnetcdf-dev \
      libnetcdff-dev \
      libnetcdff7 \
      libxml2-utils \
      pylint \
      cmake \
      curl \
      pkg-config && \
      rm -rf /var/lib/apt/lists && \
      update-alternatives --install /usr/bin/python python /usr/bin/python3 10 && \
      update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 10 && \
      update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-9 10 && \
      update-alternatives --install /usr/bin/gfortran gfortran /usr/bin/gfortran-9 10

ENV USER=root
ENV CIME_MODEL=cesm
