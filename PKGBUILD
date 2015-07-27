# Maintainer: Dan Ziemba <zman0900@gmail.com>
# Contributor: Mark Weiman <mark dot weiman at markzz dot com>
# Contributor: Florian Pritz <bluewind@xinu.at>
# Contributor: Christoph Vigano <mail at cvigano dot de>
# Contributor: Biru Ionut <ionut@archlinux.ro>
# Contributor: Pierre Schmitz <pierre@archlinux.de>
# Contributor: Mikko Seppälä <t-r-a-y@mbnet.fi>

_pkgbasename=gnutls
pkgname=lib32-${_pkgbasename}28
pkgver=3.3.16
pkgrel=1
pkgdesc="A library which provides a secure layer over a reliable transport layer (32-bit, legacy version)"
arch=('x86_64')
license=('GPL3' 'LGPL2.1')
url="http://gnutls.org/"
depends=('lib32-zlib' 'lib32-nettle4>=2.7.1-3' 'lib32-p11-kit' 'lib32-libtasn1' $_pkgbasename)
makedepends=('gcc-multilib' 'lib32-libidn')
source=(ftp://ftp.gnutls.org/gcrypt/gnutls/v3.3/${_pkgbasename}-${pkgver}.tar.xz{,.sig})
md5sums=('c2143db71a57248f7bdb2fb6acd6b567'
         'SKIP')
validpgpkeys=(1F42418905D8206AA754CCDC29EE58B996865171)

build() {
  export CC="gcc -m32"
  export CXX="g++ -m32"
  export PKG_CONFIG_PATH="/usr/lib32/pkgconfig"

  cd ${srcdir}/${_pkgbasename}-${pkgver}

  # build fails without --disable-hardware-acceleration because of assembler errors
  ./configure --prefix=/usr --libdir=/usr/lib32 \
    --with-zlib \
    --disable-static \
    --disable-guile \
    --disable-valgrind-tests --disable-hardware-acceleration \
    --disable-cxx --disable-openssl-compatibility --enable-local-libopts
  make
}

check() {
  cd ${srcdir}/${_pkgbasename}-${pkgver}
  make -k check
}

package() {
  cd "${srcdir}/${_pkgbasename}-${pkgver}"
  make DESTDIR="${pkgdir}" install
  find $pkgdir

  install -m 755 -d "${pkgdir}"/usr/lib32/gnutls28
  mv "${pkgdir}"/usr/{include,lib32/gnutls28}
  rm -rf "${pkgdir}"/usr/{bin,share,lib32/{*.so,pkgconfig}}
}
