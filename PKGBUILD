# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Sébastien "Seblu" Luttringer
# Maintainer: Tobias Powalowski <tpowa@archlinux.org>
# Contributor: Bartłomiej Piotrowski <bpiotrowski@archlinux.org>
# Contributor: Allan McRae <allan@archlinux.org>
# Contributor: judd <jvinet@zeroflux.org>
# Contributor: Pellegrino Prevete (dvorak) <pellegrinoprevete@gmail.com>
# Contributor: Truocolo <truocolo@aol.com>

pkgname=coreutils
pkgver=9.4
pkgrel=2
_pkgdesc=(
  'The basic file, shell and'
  'text manipulation utilities of the'
  'GNU operating system')
pkgdesc="${_pkgdesc[*]}"
arch=(
  'x86_64'
  'arm'
  'aarch64'
)
license=(
  'GPL-3.0-or-later'
  'GFDL-1.3-or-later'
)
url="https://www.gnu.org/software/${pkgname}"
depends=(
  'glibc'
  'acl'
  'attr'
  'gmp'
  'libcap'
  'openssl'
)
source=(
  "https://ftp.gnu.org/gnu/$pkgname/$pkgname-$pkgver.tar.xz"{,.sig}
)
validpgpkeys=(
   # Pádraig Brady
  '6C37DC12121A5006BC1DB804DF6FD971306037D9'
)
sha256sums=(
  'ea613a4cf44612326e917201bbbcdfbd301de21ffc3b59b6e5c07e040b275e52'
  'SKIP'
)

prepare() {
  cd \
    "${pkgname}-${pkgver}"
  # apply patch from the source array 
  # (should be a pacman feature)
  local \
    src
  for src in "${source[@]}"; do
    src="${src%%::*}"
    src="${src##*/}"
    [[ $src = *.patch ]] || \
      continue
    echo \
      "Applying patch $src..."
    patch \
      -Np1 < \
        "../${src}"
  done
}

build() {
  local \
    _cflags=()
    _configure_opts=()
  cd \
    "${pkgname}-${pkgver}"
  _cflags=(
    -Wno-implicit-function-declaration 
    -Wno-error="implicit-function-declaration"
  )
  _configure_opts=(
    --prefix=/usr
    --libexecdir=/usr/lib
    --with-openssl
    gl_cv_host_operating_system="$( \
      uname \
        -o)"
  )
  [[ "${CARCH}" == "arm" ]] && \
    _configure_opts+=(
      # --enable-no-install-program=groups,hostname,kill,uptime
      # --enable-no-install-program=pinky,df,users,who,uptime
      ac_cv_func_getpass="yes"
      --disable-year2038
      --enable-single-binary=symlinks
      --with-gmp
      --disable-xattr
    )
  export \
    CFLAGS="${CFLAGS} ${_cflags[*]}"
  ./configure \
    "${_configure_opts[@]}"
  make
}

check() {
  cd \
    "${pkgname}-${pkgver}"
  make \
    check
}

package() {
  cd \
    "${pkgname}-${pkgver}"
  make \
    DESTDIR="${pkgdir}" \
    install
}

# vim:set sw=2 sts=-1 et:
