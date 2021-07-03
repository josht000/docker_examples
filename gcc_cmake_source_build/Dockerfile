# Build GCC from source. Find GCC Releases here:
# https://github.com/gcc-mirror/gcc/releases

FROM ubuntu:18.04

RUN apt update && apt-get install -y \
    	apt-transport-https \
		aria2 \
		bison \
		build-essential \
		# cmake \
		curl \
		flex \
        wget \
		libfl-dev \
		libgmp-dev \
		libmpc-dev \
		libmpfr-dev \
		libz3-dev

WORKDIR /tmp
RUN wget -q https://github.com/gcc-mirror/gcc/archive/refs/tags/releases/gcc-11.1.0.tar.gz && tar -xf gcc-11.1.0.tar.gz && rm -f *.gz
RUN cd /tmp/gcc-releases-gcc-11.1.0 && ./contrib/download_prerequisites && cd /tmp && mkdir build

# C / C++ ONLY! 
# Notes:
# --disable-multilib : Specify that multiple target libraries to support different target variants, calling conventions, etc. should not be built. 
# --enable-checking=release : controls performing internal consistency checks in the compiler. 
# --disable-bootstrap : By default, gcc is built three times. With the --disable-bootstrap option it’s built just once resulting in a much quicker build process. 
# --enable-gold : use the gold linker rather than the default ld linker because it tends to be faster for C++ projects.
RUN cd /tmp/build && ../gcc-releases-gcc-11.1.0/configure \
        -v --build=x86_64-linux-gnu \
        --host=x86_64-linux-gnu \
        --target=x86_64-linux-gnu \
        --prefix=/usr/local \
        --enable-checking=release \
        --enable-languages=c,c++ \
        --disable-multilib \
        --disable-bootstrap \
        --enable-gold \
        --program-suffix=-11.1 \ 
        && make -j16 && make install-strip && cd /tmp && rm -rf /tmp/*

# RUN update-alternatives --install /usr/local/gcc gcc /usr/local/bin/gcc-11.1 60 --slave /usr/local/bin/g++ g++-11 /usr/bin/g++-11
RUN cd /usr/bin \
    && rm gcc && rm g++ && rm x86_64-linux-gnu-gcc && rm x86_64-linux-gnu-g++ && rm x86_64-linux-gnu-gcc-nm \
    && ln -s /usr/local/bin/gcc-11.1 /usr/bin/gcc \
    && ln -s /usr/local/bin/g++-11.1 /usr/bin/g++ \
    && ln -s /usr/local/bin/x86_64-linux-gnu-gcc-11.1 /usr/bin/x86_64-linux-gnu-gcc \
    && ln -s /usr/local/bin/x86_64-linux-gnu-g++-11.1 /usr/bin/x86_64-linux-gnu-g++ \
    && ln -s /usr/local/bin/x86_64-linux-gnu-gcc-nm-11.1 /usr/bin/x86_64-linux-gnu-gcc-nm

# RUN update-alternatives --install /usr/gcc gcc /usr/bin/gcc-7 60 --slave /usr/bin/g++ g++-7 /usr/bin/g++-7

# RUN echo which gcc-11.1
# To let CMake know
# export CC=/usr/bin/gcc-11.1
# export CXX=/usr/bin/g++-11.1

# Fastest GCC 11 performace : -O3 -march=native -flto # enables tons of optimizations, caters to the CPU in which program is built on, and link time optimizations. 
# Only use lto when your program is fully debugged with a thorough test suite. Then enable lto and retest with benchmarking. 

# docker build -t gcc:11.1 .