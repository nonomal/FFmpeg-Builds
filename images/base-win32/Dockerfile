ARG GH_REPO=btbn/ffmpeg-builds
FROM ghcr.io/$GH_REPO/base:latest

ENV DEBIAN_FRONTEND noninteractive

RUN \
    apt-get -y install binutils-mingw-w64-i686 gcc-mingw-w64-i686 g++-mingw-w64-i686 gfortran-mingw-w64-i686 && \
    apt-get -y clean && \
    rm /usr/lib/gcc/*-w64-mingw32/*/libstdc++*.dll* && \
    rm /usr/lib/gcc/*-w64-mingw32/*/libgcc_s* && \
    rm /usr/lib/gcc/*-w64-mingw32/*/*.dll.a && \
    rm /usr/*-w64-mingw32/lib/*.dll.a && \
    mkdir /opt/ffbuild

RUN rustup target add i686-pc-windows-gnu

ADD toolchain.cmake /toolchain.cmake
ADD cross.meson /cross.meson

ENV FFBUILD_TARGET_FLAGS="--pkg-config=pkg-config --cross-prefix=i686-w64-mingw32- --arch=i686 --target-os=mingw32" \
    FFBUILD_TOOLCHAIN=i686-w64-mingw32 \
    FFBUILD_CROSS_PREFIX=i686-w64-mingw32- \
    FFBUILD_RUST_TARGET=i686-pc-windows-gnu \
    FFBUILD_PREFIX=/opt/ffbuild \
    FFBUILD_CMAKE_TOOLCHAIN=/toolchain.cmake \
    PKG_CONFIG=pkg-config \
    PKG_CONFIG_LIBDIR=/opt/ffbuild/lib/pkgconfig:/opt/ffbuild/share/pkgconfig \
    CFLAGS="-static-libgcc -static-libstdc++ -I/opt/ffbuild/include -O2 -pipe -D_FORTIFY_SOURCE=2 -fstack-protector-strong" \
    CXXFLAGS="-static-libgcc -static-libstdc++ -I/opt/ffbuild/include -O2 -pipe -D_FORTIFY_SOURCE=2 -fstack-protector-strong" \
    LDFLAGS="-static-libgcc -static-libstdc++ -L/opt/ffbuild/lib -O2 -pipe -fstack-protector-strong" \
    DLLTOOL="i686-w64-mingw32-dlltool" \
    STAGE_CFLAGS="-fno-semantic-interposition" \
    STAGE_CXXFLAGS="-fno-semantic-interposition"
