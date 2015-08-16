# $Id: PKGBUILD 186340 2013-05-25 02:56:30Z foutrelis $
# Maintainer: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Tobias Kieslich <tobias@justdreams.de>
# Contributor: tobias <tobias@archlinux.org>

pkgname=inkscape-pruned
_pkgname=inkscape
pkgver=0.48.4
pkgrel=1
pkgdesc='Inkscape without inkview, locales nor tutorials'
url='http://inkscape.sourceforge.net/'
license=('GPL' 'LGPL')
arch=('i686' 'x86_64')
makedepends=('boost' 'intltool')
provides=("$_pkgname")
conflicts=("$_pkgname")
depends=('gc' 'gsl' 'gtkmm' 'gtkspell' 'imagemagick' 'libxslt' 'poppler-glib>=0.22.3' 'popt'
         'python2' 'desktop-file-utils' 'hicolor-icon-theme')
optdepends=('pstoedit: latex formulas'
            'texlive-core: latex formulas'
            'python2-numpy: some extensions'
            'python2-lxml: some extensions and filters'
            'pyxml: some extensions'
            'uniconvertor: reading/writing to some proprietary formats')
options=('!libtool')
source=("http://downloads.sourceforge.net/project/${_pkgname}/${_pkgname}/${pkgver}/${_pkgname}-${pkgver}.tar.bz2"
        'spuriouscomma.patch')
sha1sums=('5f26f6ad191d1e7c2a9fb69a438722beb172224c'
          '7d1d5a6d1d2b0926721a994d5889c52890fc57c1')

install=install

prepare() {
	cd "${srcdir}/${_pkgname}-${pkgver}"
	patch -p0 -i ../spuriouscomma.patch
	sed -i 's|/usr/bin/python\>|/usr/bin/python2|g' cxxtest/*.py
	sed -i 's|/usr/bin/env python\>|/usr/bin/env python2|g' share/*/{test/,}*.py
	sed -i 's|"python" },|"python2" },|g' src/extension/implementation/script.cpp
	sed -i 's|python -c|python2 -c|g' configure share/extensions/uniconv*.py
	sed -i 's|"python"|"python2"|g' src/main.cpp
	sed -i '/^#include <g.kmm/i #include <glibmm.h>' src/*{,/*{,/*{,/*}}}.{h,cpp}
}

build() {
	cd "${srcdir}/${_pkgname}-${pkgver}"
	./configure \
		--prefix=/usr \
		--with-python \
		--with-perl \
		--enable-lcms \
		--enable-poppler-cairo \
		--disable-dependency-tracking
	make
}

package() {
	cd "${srcdir}/${_pkgname}-${pkgver}"
	make DESTDIR="${pkgdir}" install

    rm -r "$pkgdir/usr/bin/inkview"
    rm -r "$pkgdir/usr/share/locale/"
    rm -r "$pkgdir/usr/share/inkscape/tutorials/"
}
