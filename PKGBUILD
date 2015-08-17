# Maintainer: Christian Hesse <mail@eworm.de>
# Contributor: Lukas Fleischer <archlinux at cryptocrack dot de>
# Contributor: Andrea Zucchelli <zukka77@gmail.com>
# Contributor: SanskritFritz (gmail)

pkgname=tinyproxy18-git
pkgver=1.8.4.r0.g1115251
pkgrel=1
pkgdesc='A light-weight HTTP proxy daemon for POSIX operating systems - git checkout, 1.8 branch'
arch=('i686' 'x86_64')
url='https://banu.com/tinyproxy/'
license=('GPL')
makedepends=('asciidoc' 'git')
provides=('tinyproxy')
conflicts=('tinyproxy' 'tinyproxy-git')
install=tinyproxy.install
backup=('etc/tinyproxy/tinyproxy.conf')
source=('git://git.banu.com/tinyproxy.git#branch=1.8'
	'config.patch'
	'tinyproxy.tmpfiles.conf'
	'tinyproxy.service')

pkgver() {
	cd tinyproxy/

	if GITTAG="$(git describe --abbrev=0 --tags 2>/dev/null)"; then
		echo "$(sed -e "s/^${pkgname%%-git}//" -e 's/^[-_/a-zA-Z]\+//' -e 's/[-_+]/./g' <<< ${GITTAG}).r$(git rev-list --count ${GITTAG}..).g$(git log -1 --format="%h")"
	else
		echo "0.r$(git rev-list --count master).g$(git log -1 --format="%h")"
	fi
}

prepare() {
	cd tinyproxy/

	patch -Np0 -i ${srcdir}/config.patch
}

build() {
	cd tinyproxy/

	./autogen.sh
	./configure \
		--prefix=/usr \
		--sbindir=/usr/bin \
		--sysconfdir=/etc/tinyproxy \
		--localstatedir=/var \
		--enable-transparent
        make
}

package() {
	cd tinyproxy/

	make DESTDIR="${pkgdir}" install

	install -Dm0644 "${srcdir}/tinyproxy.tmpfiles.conf" "${pkgdir}/usr/lib/tmpfiles.d/tinyproxy.conf"
	install -Dm0644 "${srcdir}/tinyproxy.service" "${pkgdir}/usr/lib/systemd/system/tinyproxy.service"
}
sha256sums=('SKIP'
            '3cbb1591d67c18d52bdf5d36396902a9161421351ddabcc72df982cc2c8a426b'
            '95de6e8c332dc60c466889d8c268c345a66848c67673103d1739bc2c6e69f827'
            '78f93d5a0880a3debcf745ac3aa0f1ed9613a5f860be1c8995244270d46c85ab')
