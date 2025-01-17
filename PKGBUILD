#!/bin/bash

# Disable various shellcheck rules that produce false positives in this file.
# Repository rules should be added to the .shellcheckrc file located in the
# repository root directory, see https://github.com/koalaman/shellcheck/wiki
# and https://archiv8.github.io for further information.
# shellcheck disable=SC2034,SC2154
# [ToDo]: Add files: User documentation
# [ToDo]: Add files: Tooling
# [FixMe]: Namcap warnings and errors

# Maintainer: Ross Clark <archiv8@artisteducator.com>
# Contributor: Ross Clark <archiv8@artisteducator.com>

pkgname=uavs3d
pkgver=1.1.r34.g7b1dd73
pkgrel=1
pkgdesc="An AVS3 decoder supporting AVS3-P2 baseline profile (git version)"
arch=("x86_64")
url="https://github.com/uavs3/uavs3d/"
license=("BSD")
depends=(
    "glibc"
)
makedepends=(
    "git"
    "cmake"
)
provides=(
    "uavs3d"
)
conflicts=(
    "uavs3d"
)
source=(
    "git+https://github.com/uavs3/uavs3d.git"

    "010-uavs3d-10bit.patch"
)
sha256sums=("SKIP"
    "44c5880aad6ae7891c761dfbcc3ea6c0304e5272953275167054904349bd901f")

prepare() {
    cp -a uavs3d uavs3d-10bit
    patch -d uavs3d-10bit -Np1 -i "${srcdir}/010-uavs3d-10bit.patch"
}

pkgver() {
    git -C uavs3d describe --long --tags | sed "s/\([^-]*-g\)/r\1/;s/-/./g;s/^v//"
}

build() {
    cd uavs3d
    cmake -B build/linux -S . \
        -DCMAKE_BUILD_TYPE:STRING="None" \
        -DCMAKE_INSTALL_PREFIX:PATH="/usr" \
        -DCMAKE_SKIP_RPATH:BOOL="YES" \
        -DBUILD_SHARED_LIBS:BOOL="ON" \
        -Wno-dev
    make -C build/linux

    cd "${srcdir}/uavs3d-10bit"
    cmake -B build/linux -S . \
        -DCMAKE_BUILD_TYPE:STRING="None" \
        -DCMAKE_INSTALL_PREFIX:PATH="/usr" \
        -DCMAKE_SKIP_RPATH:BOOL="YES" \
        -DBUILD_SHARED_LIBS:BOOL="ON" \
        -Wno-dev
    make -C build/linux
}

package() {
    make -C uavs3d/build/linux DESTDIR="$pkgdir" install
    make -C uavs3d-10bit/build/linux DESTDIR="$pkgdir" install
    install -D -m755 uavs3d/build/linux/uavs3dec -t "${pkgdir}/usr/bin"
    install -D -m755 uavs3d-10bit/build/linux/uavs3dec "${pkgdir}/usr/bin/uavs3dec-10bit"
    install -D -m644 uavs3d/COPYING "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
