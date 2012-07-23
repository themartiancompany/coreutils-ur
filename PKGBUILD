# Maintainer: Allan McRae <allan@archlinux.org>
# Contributor: judd <jvinet@zeroflux.org>

pkgname=coreutils
pkgver=8.17
pkgrel=3
pkgdesc="The basic file, shell and text manipulation utilities of the GNU operating system"
arch=('i686' 'x86_64')
license=('GPL3')
url="http://www.gnu.org/software/coreutils"
groups=('base')
depends=('glibc' 'pam' 'acl' 'gmp' 'libcap')
replaces=('mktemp')
backup=('etc/pam.d/su')
install=${pkgname}.install
options=('!emptydirs')
source=(ftp://ftp.gnu.org/gnu/$pkgname/$pkgname-$pkgver.tar.xz{,.sig}
	coreutils-pam.patch
	0001-ls-color-each-symlink-to-relative-name-in-properly.patch
	su.pam)
md5sums=('bbda656ce8ca2c6903948f9faa204ba3'
         'ebecd29b095aa21b0b2f833f1ec20d70'
         'aad79a2aa6d566c375d7bdd1b0767278'
         'd7c691898a695a6284a927e6a9426fe4'
         'fa85e5cce5d723275b14365ba71a8aad')

build() {
  cd ${srcdir}/${pkgname}-${pkgver}

  # added su wheel group pam patch (from fedora git)
  patch -Np1 -i ${srcdir}/coreutils-pam.patch

  # fix coloring for symlinks in /
  # upstream commit 6124a3842dfa8484b52e067a8ab8105c3875a4f7
  patch -Np1 -i $srcdir/0001-ls-color-each-symlink-to-relative-name-in-properly.patch

  autoreconf -v
  ./configure --prefix=/usr --libexecdir=/usr/lib \
              --enable-install-program=su \
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
  install -dm755 ${pkgdir}/{bin,usr/sbin}

  # binaries required by FHS
  _fhs=('cat' 'chgrp' 'chmod' 'chown' 'cp' 'date' 'dd' 'df' 'echo' 'false'
        'ln' 'ls' 'mkdir' 'mknod' 'mv' 'pwd' 'rm' 'rmdir' 'stty' 'su' 'sync'
        'true' 'uname')
  mv ${_fhs[@]} ${pkgdir}/bin

  mv chroot ${pkgdir}/usr/sbin
  install -Dm644 ${srcdir}/su.pam ${pkgdir}/etc/pam.d/su
}
