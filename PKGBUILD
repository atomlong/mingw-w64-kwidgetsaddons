# Maintainer: Mladen Milinkovic <maxrd2@smoothware.net>

pkgname=mingw-w64-kwidgetsaddons
pkgver=5.85.0
pkgrel=1
arch=(any)
pkgdesc="Addons to QtWidgets (mingw-w64)"
license=("LGPL")
depends=(mingw-w64-qt5-base)
groups=(mingw-w64-kf5)
makedepends=(mingw-w64-cmake mingw-w64-extra-cmake-modules mingw-w64-qt5-tools)
options=(staticlibs !strip !buildflags)
url="https://community.kde.org/Frameworks"
source=("http://download.kde.org/stable/frameworks/${pkgver%.*}/kwidgetsaddons-${pkgver}.tar.xz"{,.sig})
sha256sums=('61646fff0c2349bacbf3042f3aaaab460b8939b35157a72973885099d8160490'
            'SKIP')
validpgpkeys=(53E6B47B45CEA3E0D5B7457758D0EE648A48B3BB) # David Faure <faure@kde.org>

_architectures="i686-w64-mingw32 x86_64-w64-mingw32"

build() {
	cd kwidgetsaddons-$pkgver
	for _arch in ${_architectures}; do
    unset LDFLAGS
    mkdir "build-${_arch}" && pushd "build-${_arch}"
    ${_arch}-cmake \
      -DCMAKE_BUILD_TYPE=Release \
      -DKDE_INSTALL_USE_QT_SYS_PATHS=ON \
      -DBUILD_TESTING=OFF \
      ..
    make
    popd
  done
}

package() {
	for _arch in ${_architectures}; do
    cd "${srcdir}/${pkgname#mingw-w64-}-$pkgver/build-${_arch}"
    make DESTDIR="$pkgdir" install
    find "$pkgdir/usr/${_arch}" -name '*.dll' -exec ${_arch}-strip --strip-unneeded {} \;
    find "$pkgdir/usr/${_arch}" -name '*.a' -o -name '*.dll' | xargs ${_arch}-strip -g
    rm -rf "$pkgdir/usr/${_arch}/share"
  done
}
