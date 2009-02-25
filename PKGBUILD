# Maintainer: Andreas Radke <andyrtr@archlinux.org>
# Contributor: judd <jvinet@zeroflux.org>

pkgname=coreutils
pkgver=7.1
pkgrel=1.1
pkgdesc="The basic file, shell and text manipulation utilities of the GNU operating system"
arch=(i686 x86_64)
license=('GPL3')
url="http://www.gnu.org/software/coreutils"
groups=('base')
depends=('glibc>=2.9-4' 'shadow>=4.1.2.1-2' 'pam>=1.0.3' 'acl>=2.2.47-1' 'gmp>=4.2.4')
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
md5sums=('cbb2b3d1718ee1237b808e00b5c11b1e'
         'aa3b805025b4f0ce682db2fa2fae8ead'
         '18d3ba178e2691242287b59bd81aedb9'
         '51a063ea50e4a8a1be7fcf170c7e27e3'
         '92fefe96cfb43eb19e25fbbcd3b2d023'
         'fa85e5cce5d723275b14365ba71a8aad')

build() {
  cd ${srcdir}/${pkgname}-${pkgver}

  # only needed if new autoconf 2.62 is used
  sed -i 's/1.10a/1.10.2/' configure.ac # aclocal fix
  sed -i 's/dist-xz/dist-lzma/' configure.ac
  autoreconf

  # added pam patch and i18n patch from fedora cvs
  patch -Np1 -i ../coreutils-pam.patch || return 1
  patch -Np1 -i ../coreutils-i18n.patch || return 1
  patch -Np1 -i ../coreutils-6.10-configuration.patch || return 1

  # from gentoo portage
  patch -Np1 -i ../coreutils-uname.patch || return 1

  ./configure --prefix=/usr \
	--enable-install-program=su \
	--enable-pam ac_cv_func_openat=no
  make || return 1
  make DESTDIR=${pkgdir} install || return 1

  rm -f ${pkgdir}/usr/bin/hostname ${pkgdir}/usr/share/man/man1/hostname.1 || return 1
  rm -f ${pkgdir}/usr/bin/uptime ${pkgdir}/usr/share/man/man1/uptime.1 || return 1
  rm -f ${pkgdir}/usr/bin/groups ${pkgdir}/usr/share/man/man1/groups.1 || return 1
  rm -f ${pkgdir}/usr/bin/kill ${pkgdir}/usr/share/man/man1/kill.1|| return 1
  cd ${pkgdir}/usr/bin
  mkdir -p ${pkgdir}/bin ${pkgdir}/sbin ${pkgdir}/usr/sbin
  mv su date echo false pwd stty true uname cat tr cut readlink ../../bin
  mv dd cp df du ln ls mv rm dir sync vdir chgrp chmod chown ../../bin
  mv mkdir mknod rmdir shred touch mkfifo dircolors install sleep ../../bin
  mv chroot ../sbin
  ln -sf test [
  ln -sf /bin/sleep ${pkgdir}/usr/bin/sleep
  install -D -m644 $startdir/src/su ${pkgdir}/etc/pam.d/su

  ls -lha ${pkgdir}/bin/su
  chmod -v 4555 ${pkgdir}/bin/su

  rm -f ${pkgdir}/usr/share/info/dir
}
