# Maintainer: Allan McRae <allan@archlinux.org>
# Contributor: judd <jvinet@zeroflux.org>

pkgname=coreutils
pkgver=8.19
pkgrel=1
pkgdesc="The basic file, shell and text manipulation utilities of the GNU operating system"
arch=('i686' 'x86_64')
license=('GPL3')
url="http://www.gnu.org/software/coreutils"
groups=('base')
depends=('glibc' 'pam' 'acl' 'gmp' 'libcap')
install=${pkgname}.install
source=(ftp://ftp.gnu.org/gnu/$pkgname/$pkgname-$pkgver.tar.xz{,.sig})
md5sums=('1a01231a2f3ed37c0efc073ccdda9375'
         '7f564749d834397aa67f0f05bacb62d5')

build() {
  cd ${srcdir}/${pkgname}-${pkgver}

  ./configure --prefix=/usr --libexecdir=/usr/lib \
              --enable-no-install-program=groups,hostname,kill,uptime \
              --enable-pam
  make
}

check() {
  cd ${srcdir}/${pkgname}-${pkgver}
  make RUN_EXPENSIVE_TESTS=yes check
}

package() {
  cd ${srcdir}/${pkgname}-${pkgver}
  make DESTDIR=${pkgdir} install

  cd ${pkgdir}/usr/bin
  install -dm755 ${pkgdir}/bin

  # binaries required by FHS
  _fhs=('cat' 'chgrp' 'chmod' 'chown' 'cp' 'date' 'dd' 'df' 'echo' 'false'
        'ln' 'ls' 'mkdir' 'mknod' 'mv' 'pwd' 'rm' 'rmdir' 'stty' 'sync'
        'true' 'uname')
  for i in ${_fhs[@]}; do
    ln -s ../usr/bin/$i ${pkgdir}/bin/$i
  done
}
