# Maintainer: Allan McRae <allan@archlinux.org>
# Contributor: judd <jvinet@zeroflux.org>

pkgname=coreutils
pkgver=8.2
pkgrel=1
pkgdesc="The basic file, shell and text manipulation utilities of the GNU operating system"
arch=('i686' 'x86_64')
license=('GPL3')
url="http://www.gnu.org/software/coreutils"
groups=('base')
depends=('glibc' 'shadow' 'pam' 'acl' 'gmp' 'libcap')
provides=('mktemp')
conflicts=('mktemp')
replaces=('sh-utils' 'fileutils' 'textutils' 'mktemp')
backup=('etc/pam.d/su')
install=${pkgname}.install
options=('!emptydirs')
source=(ftp://ftp.gnu.org/gnu/$pkgname/$pkgname-$pkgver.tar.gz
	coreutils-i18n.patch
	coreutils-uname.patch
	coreutils-pam.patch
	coreutils-6.10-configuration.patch
	su)
md5sums=('dfb0d3dbc5f555386339f4f74621cda0'
         '0ccdad826d251515b61d4b60da93a7e5'
         'c4fcca138b6abf6d443d48a6f0cd8833'
         '7efee4d5b3653711f9e229493316841c'
         '8db636d7bf3c05068d96f88cb84bdf9d'
         'fa85e5cce5d723275b14365ba71a8aad')

build() {
  cd ${srcdir}/${pkgname}-${pkgver}

  # added pam patch and i18n patch from fedora cvs
  patch -Np1 -i $srcdir/coreutils-pam.patch || return 1
  patch -Np1 -i $srcdir/coreutils-i18n.patch || return 1
  patch -Np1 -i $srcdir/coreutils-6.10-configuration.patch || return 1

  # from gentoo portage
  patch -Np1 -i $srcdir/coreutils-uname.patch || return 1

  autoreconf -v
  ./configure --prefix=/usr \
              --enable-install-program=su \
              --enable-pam ac_cv_func_openat=no || return 1
  make || return 1
  make DESTDIR=${pkgdir} install || return 1

  for f in hostname uptime groups kill; do
    rm -f ${pkgdir}/usr/{bin/${f},share/man/man1/${f}.1} || return 1
  done
  
  cd ${pkgdir}/usr/bin
  install -dm755 ${pkgdir}/{bin,usr/sbin}
  
  # binaries required by FHS
  _fhs="cat chgrp chmod chown cp date dd df echo false ln ls \
        mkdir mknod mv pwd rm rmdir stty su sync true uname"
  mv ${_fhs} $pkgdir/bin
  ls -lha ${pkgdir}/bin/su
  chmod -v 4555 ${pkgdir}/bin/su

  # binaries required by various scripts
  _bin="cut dir dircolors du install mkfifo readlink shred \
        sleep touch tr vdir"
  mv ${_bin} $pkgdir/bin
  ln -sf /bin/sleep ${pkgdir}/usr/bin/sleep
    
  mv chroot $pkgdir/usr/sbin
  ln -sf test [
  install -Dm644 $srcdir/su ${pkgdir}/etc/pam.d/su
}
