# Maintainer: Allan McRae <allan@archlinux.org>
# Contributor: judd <jvinet@zeroflux.org>

pkgname=coreutils
pkgver=7.3
pkgrel=1
pkgdesc="The basic file, shell and text manipulation utilities of the GNU operating system"
arch=('i686' 'x86_64')
license=('GPL3')
url="http://www.gnu.org/software/coreutils"
groups=('base')
depends=('glibc>=2.9-4' 'shadow>=4.1.3.1-1' 'pam>=1.0.4' 'acl>=2.2.47-2' 'gmp>=4.2.4')
provides=('mktemp')
conflicts=('mktemp')
replaces=('sh-utils' 'fileutils' 'textutils' 'mktemp')
backup=('etc/pam.d/su')
install=${pkgname}.install
options=('!emptydirs' '!makeflags')
source=(ftp://ftp.gnu.org/gnu/$pkgname/$pkgname-$pkgver.tar.gz
	coreutils-i18n.patch
	coreutils-uname.patch
	coreutils-pam.patch
	coreutils-6.10-configuration.patch
	coreutils-7.3-automake.patch
	su)
md5sums=('b72c35e4872cf04c2b7642ef8248d0ea'
         'ed03cc1618dc815fe0a8b5975b4d6198'
         '18d3ba178e2691242287b59bd81aedb9'
         '2127eb34886396b8361c9ab39acfe01c'
         '081aeb04487d1445d0dc3fc98cf41260'
         'd35708c63564fb302e4c9208323ee823'
         'fa85e5cce5d723275b14365ba71a8aad')

build() {
  cd ${srcdir}/${pkgname}-${pkgver}

  # added pam patch and i18n patch from fedora cvs
  patch -Np1 -i $srcdir/coreutils-pam.patch || return 1
  patch -Np1 -i $srcdir/coreutils-i18n.patch || return 1
  patch -Np1 -i $srcdir/coreutils-6.10-configuration.patch || return 1

  # from gentoo portage
  patch -Np1 -i $srcdir/coreutils-uname.patch || return 1

  # we do not use the beta automake...
  patch -Np1 -i $srcdir/coreutils-7.3-automake.patch || return 1
  autoreconf -v

  ./configure --prefix=/usr \
	--enable-install-program=su \
	--enable-pam ac_cv_func_openat=no || return 1
  make || return 1
  make DESTDIR=${pkgdir} install || return 1

  rm -f ${pkgdir}/usr/bin/hostname ${pkgdir}/usr/share/man/man1/hostname.1 || return 1
  rm -f ${pkgdir}/usr/bin/uptime ${pkgdir}/usr/share/man/man1/uptime.1 || return 1
  rm -f ${pkgdir}/usr/bin/groups ${pkgdir}/usr/share/man/man1/groups.1 || return 1
  rm -f ${pkgdir}/usr/bin/kill ${pkgdir}/usr/share/man/man1/kill.1|| return 1
  cd ${pkgdir}/usr/bin
  install -dm755 ${pkgdir}/{bin,sbin,usr/sbin}
  mv su date echo false pwd stty true uname cat tr cut readlink $pkgdir/bin
  mv dd cp df du ln ls mv rm dir sync vdir chgrp chmod chown $pkgdir/bin
  mv mkdir mknod rmdir shred touch mkfifo dircolors install sleep $pkgdir/bin
  mv chroot $pkgdir/usr/sbin
  ln -sf test [
  ln -sf /bin/sleep ${pkgdir}/usr/bin/sleep
  install -Dm644 $srcdir/su ${pkgdir}/etc/pam.d/su

  ls -lha ${pkgdir}/bin/su
  chmod -v 4555 ${pkgdir}/bin/su

  rm -f ${pkgdir}/usr/share/info/dir
  gzip -9 ${pkgdir}/usr/share/info/*
}
