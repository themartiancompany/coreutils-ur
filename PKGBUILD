# Maintainer:  Sébastien "Seblu" Luttringer
# Maintainer:  Bartłomiej Piotrowski <bpiotrowski@archlinux.org>
# Contributor: Allan McRae <allan@archlinux.org>
# Contributor: judd <jvinet@zeroflux.org>

pkgname=coreutils
pkgver=8.22
pkgrel=3
pkgdesc='The basic file, shell and text manipulation utilities of the GNU operating system'
arch=('i686' 'x86_64')
license=('GPL3')
url='http://www.gnu.org/software/coreutils'
groups=('base')
depends=('glibc' 'pam' 'acl' 'gmp' 'libcap' 'openssl')
install=$pkgname.install
source=(ftp://ftp.gnu.org/gnu/$pkgname/$pkgname-$pkgver.tar.xz{,.sig}
        coreutils-8.22-shuf-segfault.patch)
md5sums=('8fb0ae2267aa6e728958adc38f8163a2'
         'SKIP'
         '94f7e6f373f37beb236caabed8fcdb52')

prepare() {
  cd $pkgname-$pkgver
  patch -p1 -i ../coreutils-8.22-shuf-segfault.patch
}

build() {
  cd $pkgname-$pkgver
  ./configure --prefix=/usr --libexecdir=/usr/lib --with-openssl \
    --enable-no-install-program=groups,hostname,kill,uptime
  make
}

check() {
  cd $pkgname-$pkgver
  make RUN_EXPENSIVE_TESTS=yes check
}

package() {
  cd $pkgname-$pkgver
  make DESTDIR="$pkgdir" install
}
