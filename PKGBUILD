# Maintainer: Sébastien "Seblu" Luttringer
# Contributor: Bartłomiej Piotrowski <bpiotrowski@archlinux.org>
# Contributor: Allan McRae <allan@archlinux.org>
# Contributor: judd <jvinet@zeroflux.org>

pkgname=coreutils
pkgver=8.23
pkgrel=1
pkgdesc='The basic file, shell and text manipulation utilities of the GNU operating system'
arch=('i686' 'x86_64')
license=('GPL3')
url='http://www.gnu.org/software/coreutils'
groups=('base')
depends=('glibc' 'pam' 'acl' 'gmp' 'libcap' 'openssl')
install=$pkgname.install
source=("ftp://ftp.gnu.org/gnu/$pkgname/$pkgname-$pkgver.tar.xz"{,.sig})
validpgpkeys=('6C37DC12121A5006BC1DB804DF6FD971306037D9') # Pádraig Brady
md5sums=('abed135279f87ad6762ce57ff6d89c41'
         'SKIP')

#prepare() {
#  cd $pkgname-$pkgver
#}

build() {
  cd $pkgname-$pkgver
  ./configure \
      --prefix=/usr \
      --libexecdir=/usr/lib \
      --with-openssl \
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

# vim:set ts=2 sw=2 et:
