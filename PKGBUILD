# SPDX-License-Identifier: AGPL-3.0

#    -----------------------------------------------------
#    Copyright © 2024, 2025, 2026  Pellegrino Prevete
#
#    All rights reserved
#    -----------------------------------------------------
#
#    This program is free software: you can redistribute
#    it and/or modify it under the terms of the
#    GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of
#    the License, or (at your option) any later version.
#
#    This program is distributed in the hope that it
#    will be useful, but WITHOUT ANY WARRANTY;
#    without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
#    See the GNU Affero General Public License for
#    more details.
#
#    You should have received a copy of the
#    GNU Affero General Public License
#    along with this program.
#    If not, see <https://www.gnu.org/licenses/>.

# Maintainers:
#   Truocolo
#     <truocolo@aol.com>
#     <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
#   Pellegrino Prevete (dvorak)
#     <pellegrinoprevete@gmail.com>
#     <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Contributors:
#   Sébastien "Seblu" Luttringer
#   Tobias Powalowski
#     <tpowa@archlinux.org>
#   Bartłomiej Piotrowski
#     <bpiotrowski@archlinux.org>
#   Allan McRae
#     <allan@archlinux.org>
#   judd
#     <jvinet@zeroflux.org>

_os="$(
  uname \
    -o)"
if [[ "${_os}" == "Android" ]]; then
  _libc="ndk-sysroot"
  _compiler="clang"
  _libcompiler="llvm-libs"
elif [[ "${_os}" == "GNU/Linux" ]]; then
  _libc="glibc"
  _compiler="gcc"
  _libcompiler="libgcc"
elif [[ "${_os}" == "Msys" ]]; then
  _libc="msys2-w32api-runtime"
  _libc_headers="msys2-w32api-headers"
  _compiler="gcc"
  _libcompiler="gcc-libs"
  _sh="sh"
else
  _msg=(
    "Unknown os '${_os}'."
  )
  msg \
    "${_msg[*]}"
  _libc="msys2-w32api-runtime"
  _libc_headers="msys2-w32api-headers"
  _compiler="gcc"
  _libcompiler="gcc-libs"
  _sh="sh"
fi
_evmfs_available="$(
  command \
    -v \
    "evmfs" || \
    true)"
if [[ ! -v "_evmfs" ]]; then
  if [[ "${_evmfs_available}" != "" ]]; then
    _evmfs="true"
  elif [[ "${_evmfs_available}" == "" ]]; then
    _evmfs="false"
  fi
fi
if [[ ! -v "_ns" ]]; then
  _ns="gnu"
  _ns="themartiancompany"
fi
if [[ ! -v "_git" ]]; then
  _git="false"
fi
if [[ ! -v "_offline" ]]; then
  _offline="false"
fi
if [[ ! -v "_git_service" ]]; then
  _git_service="github"
fi
if [[ ! -v "_archive_format" ]]; then
  if [[ "${_git}" == "true" ]]; then
    if [[ "${_evmfs}" == "true" ]]; then
      _archive_format="bundle"
    elif [[ "${_evmfs}" == "false" ]]; then
      _archive_format="git"
    fi
  elif [[ "${_git}" == "false" ]]; then
    if [[ "${_ns}" == "gnu" ]]; then
      _archive_format="tar.xz"
    elif [[ "${_ns}" == "themartiancompany" ]]; then
      if [[ "${_git_service}" == "github" ]]; then
        _archive_format="zip"
      elif [[ "${_git_service}" == "gitlab" ]]; then
        _archive_format="tar.gz"
      fi
    fi
  fi
fi
_pkg=coreutils
pkgbase="${_pkg}"
pkgname=(
  "${pkgbase}"
)
_commit="c01fd163a47468a8296fb369f5233853bb551bb6"
_bundle_commit="b60a159fdc5bfcf9988d3a4cb6f53abe8ad5d35d"
pkgver=9.11
pkgrel=2
_pkgdesc=(
  'The basic file, shell and'
  'text manipulation utilities of the'
  'GNU operating system'
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  'aarch64'
  'arm'
  'armv7l'
  'armv8l'
  'i686'
  'mips'
  'pentium4'
  'powerpc'
  'x86_64'
)
license=(
  'GPL-3.0-or-later'
  'GFDL-1.3-or-later'
)
url="https://www.gnu.org/software/${_pkg}"
depends=(
  'glibc'
  'acl'
  'attr'
  'gmp'
  'libcap'
  'openssl'
)
makedepends=(
  "make"
  "${_compiler}"
)
if [[ "${_git}" == "true" ]]; then
  makedepends+=(
    "git"
  )
fi
if [[ "${_evmfs}" == "true" ]]; then
  makedepends+=(
    "evmfs"
  )
fi
if [[ "${_os}" == "Msys" ]]; then
  makedepends+=(
    "${_libc_headers}"
    "windows-default-manifest"
  )
fi
if [[ "${_tag_name}" == "pkgver" ]]; then
  _tag="${pkgver}"
elif [[ "${_tag_name}" == "commit" ]]; then
  _tag="${_commit}"
fi
_tarname="${_pkg}-${_tag}"
_tarfile="${_tarname}.${_archive_format}"
_gnu_sum=='ea613a4cf44612326e917201bbbcdfbd301de21ffc3b59b6e5c07e040b275e52'
_gnu_sig_sum="SKIP"
_bundle_sum="d549e382c34ad260b86ba63faa184fecfceb1074f6785a6a7375567ed24b4c49"
_bundle_sig_sum="995e7daaaa7b58941c7fa65ac0ba5c6df13392954508577b5bc02d9be71ab5ff"
if [[ ! -v "_http" ]]; then
  if [[ "${_ns}" == "gnu" ]]; then
    _http="https://ftp.gnu.org"
  elif [[ "${_ns}" == "themartiancompany" ]]; then
    _http="https://${_git_service}.com"
  fi
fi
_src="${_http}/${_ns}/${_pkg}/${_tarfile}"

source=(
  "${_src}"{"",".sig"}
)
validpgpkeys=(
   # Pádraig Brady
  '6C37DC12121A5006BC1DB804DF6FD971306037D9'
)
sha256sums=(
  'SKIP'
)

prepare() {
  local \
    _src
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
      --enable-no-install-program=pinky,uptime
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
