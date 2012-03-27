# Maintainer: Allan McRae <allan@archlinux.org>
# Contributor: judd <jvinet@zeroflux.org>

pkgname=coreutils
pkgver=8.16
pkgrel=2
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
	coreutils-uname.patch
	coreutils-pam.patch
	su.pam)
md5sums=('89b06f91634208dceba7b36ad1f9e8b9'
         '63158176d5bb005c6871242c940eedf1'
         'c4fcca138b6abf6d443d48a6f0cd8833'
         'aad79a2aa6d566c375d7bdd1b0767278'
         'fa85e5cce5d723275b14365ba71a8aad')

build() {
  cd ${srcdir}/${pkgname}-${pkgver}

  # added su wheel group pam patch (from fedora git)
  patch -Np1 -i ${srcdir}/coreutils-pam.patch

  # linux specific uname improvement (from gentoo portage)
  patch -Np1 -i ${srcdir}/coreutils-uname.patch

  autoreconf -v
  ./configure --prefix=/usr --libexecdir=/usr/lib/coreutils \
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
  
  # makepkg uses the full path to this...
  ln -s /usr/bin/du ${pkgdir}/bin/du

  mv chroot ${pkgdir}/usr/sbin
  install -Dm644 ${srcdir}/su.pam ${pkgdir}/etc/pam.d/su
}
