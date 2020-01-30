# Maintainer: Dominik Heidler <dominik@heidler.eu>
# Modifications: Edmunt Pienkowsky <roed@onet.eu>

pkgname=rtl_433-git
pkgver=19.08.r171.g5ef568cd
pkgrel=1
pkgdesc="Turns your Realtek RTL2832 based DVB dongle into a 433.92MHz generic data receiver"
arch=('i686' 'x86_64' 'armv6h' 'armv7h' 'aarch64')
license=('GPL')
depends=('rtl-sdr')
makedepends=('git' 'gcc' 'cmake')
optdepends=()
provides=('rtl_433')
conflicts=('rtl_433')
url="https://github.com/merbanan/rtl_433"
source=(
    'git://github.com/merbanan/rtl_433.git'
     '0001-Do-not-select-release-build-type-as-default.patch'
)
md5sums=('SKIP'
         '3a0dad691b34f809be22d692514070e2')

_gitname=rtl_433

pkgver() {
	cd ${srcdir}/${_gitname}
	git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
	cd ${srcdir}/${_gitname}
	git log -1 --format=%ct > ${srcdir}/source-date-epoch
	git am ${srcdir}/0001-Do-not-select-release-build-type-as-default.patch
}

build() {
	mkdir -p ${srcdir}/build
	cd ${srcdir}/build
	cmake -DCMAKE_INSTALL_PREFIX=/usr ../${_gitname}
	cd ${srcdir}
	env SOURCE_DATE_EPOCH=$(cat source-date-epoch) cmake --build build --clean-first
}

package() {
	cmake --build "${srcdir}/build" -- DESTDIR="${pkgdir}" install
	mv ${pkgdir}/usr/etc ${pkgdir}
}
