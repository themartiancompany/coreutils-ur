# Maintainer: judd <jvinet@zeroflux.org>
pkgname=coreutils
pkgver=6.12
pkgrel=1
pkgdesc="The basic file, shell and text manipulation utilities of the GNU operating system"
arch=(i686 x86_64)
license=('GPL3')
url="http://www.gnu.org/software/coreutils"
groups=('base')
depends=('glibc>=2.7-9' 'shadow>=4.0.18.2-2' 'pam>=1.0.1-1' 'acl>=2.2.47-1')
provides=('mktemp')
conflicts=('mktemp')
replaces=('sh-utils' 'fileutils' 'textutils' 'mktemp')
backup=('etc/pam.d/su')
options=('!emptydirs')
source=(ftp://ftp.gnu.org/gnu/$pkgname/$pkgname-$pkgver.tar.gz
	coreutils-i18n.patch
	coreutils-uname.patch
	coreutils-pam.patch
	coreutils-6.10-configuration.patch
	su)
md5sums=('2ca9ac69823dbd567b905a9e9f53c4f6'
         '64991a860ddb98a9b7a2a5a0221a399a'
         '18d3ba178e2691242287b59bd81aedb9'
         '8810a22cdc05d502a69b59511e9abf79'
         'e0f3edab474a4c96591c4f94a7962c9b'
         'fa85e5cce5d723275b14365ba71a8aad')

build() {
  cd $startdir/src/$pkgname-$pkgver

  # only needed if new autoconf 2.62 is used
  autoreconf

  # added pam patch and i18n patch from fedora cvs
  patch -Np1 -i ../coreutils-pam.patch || return 1
  patch -Np1 -i ../coreutils-i18n.patch || return 1
  patch -Np1 -i ../coreutils-6.10-configuration.patch || return 1
  # from gentoo portage
  patch -Np1 -i ../coreutils-uname.patch || return 1
  # make head and tail recognize the old syntax (eg, tail -10)
  export DEFAULT_POSIX2_VERSION=199209

  autoconf
  ./configure --prefix=/usr ac_cv_func_openat=no --enable-install-program=su --enable-pam
  make || return 1
  make DESTDIR=$startdir/pkg install
  rm -f $startdir/pkg/usr/bin/hostname $startdir/pkg/usr/share/man/man1/hostname.1 || return 1
  rm -f $startdir/pkg/usr/bin/uptime $startdir/pkg/usr/share/man/man1/uptime.1 || return 1
  rm -f $startdir/pkg/usr/bin/groups $startdir/pkg/usr/share/man/man1/groups.1 || return 1
  rm -f $startdir/pkg/usr/bin/kill $startdir/pkg/usr/share/man/man1/kill.1|| return 1
  cd $startdir/pkg/usr/bin
  mkdir -p $startdir/pkg/bin $startdir/pkg/sbin $startdir/pkg/usr/sbin
  mv su date echo false pwd stty true uname cat tr cut readlink ../../bin
  mv dd cp df du ln ls mv rm dir sync vdir chgrp chmod chown ../../bin
  mv mkdir mknod rmdir shred touch mkfifo dircolors install sleep ../../bin
  mv chroot ../sbin
  ln -sf test [
  ln -sf /bin/sleep $startdir/pkg/usr/bin/sleep
  install -D -m644 $startdir/src/su $startdir/pkg/etc/pam.d/su
}
