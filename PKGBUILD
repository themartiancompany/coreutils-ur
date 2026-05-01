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
  _git="true"
fi
if [[ ! -v "_offline" ]]; then
  _offline="false"
fi
if [[ ! -v "_tag_name" ]]; then
  _tag_name="commit"
  if [[ "${_ns}" == "gnu" ]]; then
    if [[ "${_git}" == "false" ]]; then
       _tag_name="pkgver"
    fi
  fi
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
_gnulib_commit="fb7312fa8d3df29f0ca0678f669b9a5b88a078ec"
_bundle_commit="b60a159fdc5bfcf9988d3a4cb6f53abe8ad5d35d"
_gnulib_bundle_commit="03ea6c07ce04f0ba815243191688de4ba370e95a"
pkgver=9.11
pkgrel=4
_gnulib_commit="fb7312fa8d3df29f0ca0678f669b9a5b88a078ec"
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
  "${_libc}"
  'acl'
  'attr'
  'gmp'
  'libcap'
  'openssl'
)
makedepends=(
  "autoconf"
  "automake"
  "${_compiler}"
  "${_libc}"
  "make"
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
source=()
sha256sums=()
if [[ "${_tag_name}" == "pkgver" ]]; then
  _tag="${pkgver}"
elif [[ "${_tag_name}" == "commit" ]]; then
  _tag="${_commit}"
fi
_tarname="${_pkg}-${_tag}"
_tarfile="${_tarname}.${_archive_format}"
_gnulib_tarfile="gnulib-${_gnulib_commit}.${_archive_format}"
_gnu_sum="394024eda0a5955217ceda9cd1201e65dc8fa3aa29c2951135a49521d57c3cc3"
_gnu_sig_sum="e2b9f147338cb22e41be28dcf76cd87c5197be359cc42033e66488a4a6b5c024"
_bundle_sum="d549e382c34ad260b86ba63faa184fecfceb1074f6785a6a7375567ed24b4c49"
_bundle_sig_sum="995e7daaaa7b58941c7fa65ac0ba5c6df13392954508577b5bc02d9be71ab5ff"
_github_sum="119a5ec9cb0cf5a79c2db387c911b4faa310de84f236f1e254a3b473897d30cf"
_github_sig_sum="973d6b494e33772fdb3432c867c20a204d813efa5afc2c79901c955d6ad66acf"
_gnulib_bundle_sum="693551301f6c1112d96888561c01a50c2cdd0a7aba47ba4be8d524b60ee5b006"
_gnulib_bundle_sig_sum="21106b13862b7ce9c2e5f7f6d7916801a3f21317578d2224104080765a3b0949"
if [[ ! -v "_http" ]]; then
  if [[ "${_ns}" == "gnu" ]]; then
    _http="https://ftp.gnu.org"
  elif [[ "${_ns}" == "themartiancompany" ]]; then
    _http="https://${_git_service}.com"
  fi
fi
if [[ "${_evmfs}" == "true" ]]; then
  if [[ "${_git}" == "true" ]]; then
    _sum="${_bundle_sum}"
    _sig_sum="${_bundle_sig_sum}"
  elif [[ "${_git}" == "false" ]]; then
    _sum="${_bundle_sum}"
    _sig_sum="${_bundle_sig_sum}"
  fi
elif [[ "${_evmfs}" == "false" ]]; then
  if [[ "${_git}" == "true" ]]; then
    _sum="SKIP"
    _sig_sum="SKIP"
  elif [[ "${_git}" == "false" ]]; then
    if [[ "${_git_service}" == "github" ]]; then
      _sum="${_github_sum}"
      _sig_sum="${_github_sig_sum}"
      # _sum="SKIP"
      # _sig_sum="SKIP"
    fi
  fi
fi
# Dvorak
_evmfs_ns="0x87003Bd6C074C713783df04f36517451fF34CBEf"
_evmfs_network="100"
_evmfs_address="0x69470b18f8b8b5f92b48f6199dcb147b4be96571"
_evmfs_dir="evmfs://${_evmfs_network}/${_evmfs_address}/${_evmfs_ns}"
_evmfs_uri="${_evmfs_dir}/${_sum}"
_gnulib_uri="${_evmfs_dir}/${_gnulib_bundle_sum}"
_evmfs_src="${_tarfile}::${_evmfs_uri}"
_gnulib_src="${_gnulib_tarfile}::${_gnulib_uri}"
_sig_uri="${_evmfs_dir}/${_sig_sum}"
_sig_src="${_tarfile}.sig::${_sig_uri}"
_gnulib_sig_uri="${_evmfs_dir}/${_gnulib_bundle_sig_sum}"
_gnulib_sig_src="${_gnulib_tarfile}.sig::${_gnulib_sig_uri}"
_gnulib_uri=""
_url="${_http}/${_ns}/${_pkg}"
if [[ "${_evmfs}" == "true" ]]; then
  _src="${_evmfs_src}"
  if [[ "${_git}" == "false" ]]; then
    source+=(
      "${_sig_src}"
    )
    sha256sums+=(
      "${_sig_sum}"
    )
    _sum="${_github_sum}"
  elif [[ "${_git}" == "true" ]]; then
    source+=(
      "${_sig_src}"
      "${_gnulib_src}"
      "${_gnulib_sig_src}"
    )
    sha256sums+=(
      "${_sig_sum}"
      "${_gnulib_bundle_sum}"
      "${_gnulib_bundle_sig_sum}"
    )
    _sum="${_bundle_sum}"
  fi
elif [[ "${_evmfs}" == "false" ]]; then
  if [[ "${_git}" == true ]]; then
    _src="${_tarname}::git+${_url}#${_tag_name}=${_tag}?signed"
    _gnulib_uri="${_http}/${_ns}/gnulib"
    _gnulib_src="${_gnulib_tarfile}::git+${_gnulib_uri}#${_tag_name}=${_gnulib_commit}"
    _sum="SKIP"
    source+=(
      "${_gnulib_src}"
    )
    sha256sums+=(
      'SKIP'
    )
  elif [[ "${_git}" == false ]]; then
    _uri=""
    if [[ "${_ns}" == "gnu" ]]; then
      _uri="${_http}/${_ns}/${_pkg}/${_tarfile}"
      _sum="${_gnu_sum}"
      _sig_sum="SKIP"
      source+=(
        "${_tarfile}.sig::${_uri}.sig"
      )
      sha256sums+=(
        "SKIP"
      )
    elif [[ "${_ns}" == "themartiancompany" ]]; then
      if [[ "${_git_service}" == "github" ]]; then
        if [[ "${_tag_name}" == "commit" ]]; then
          _uri="${_url}/archive/${_commit}.${_archive_format}"
          _sum="${_github_sum}"
        fi
      elif [[ "${_git_service}" == "gitlab" ]]; then
        if [[ "${_tag_name}" == "commit" ]]; then
          _uri="${_url}/-/archive/${_tag}/${_tag}.${_archive_format}"
        fi
      fi
    fi
    _src="${_tarfile}::${_uri}"
  fi
fi
source+=(
  "${_src}"
)
sha256sums+=(
  "${_sum}"
)
validpgpkeys=(
   # Pádraig Brady
  '6C37DC12121A5006BC1DB804DF6FD971306037D9'
  # Truocolo
  #   <truocolo@aol.com>
  '97E989E6CF1D2C7F7A41FF9F95684DBE23D6A3E9'
  #   <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
  'F690CBC17BD1F53557290AF51FC17D540D0ADEED'
  # Pellegrino Prevete (dvorak)
  #   <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
  '12D8E3D7888F741E89F86EE0FEC8567A644F1D16'
)

_git_unbundle() {
  local \
    _tarname="${1}" \
    _bundle \
    _repo \
    _msg=()
  _bundle="${srcdir}/${_tarname}.bundle"
  _repo="${srcdir}/${_tarname}"
  _msg=(
    "Cloning '${_bundle}' into '${_repo}'."
  )
  msg \
    "${_msg[*]}"
  git \
    clone \
      "${_bundle}" \
      "${_repo}" || \
  git \
    -C \
      "${_repo}" \
      pull || \
  true
}

prepare() {
  local \
    _src
  if [[ "${_evmfs}" == "true" ]]; then
    if [[ "${_git}" == "false" ]]; then
      ur \
        "never-gonna-give-you-up"
    elif [[ "${_git}" == "true" ]]; then
      _git_unbundle \
        "${_tarname}"
    fi
  fi
  cd \
    "${_tarname}"
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
    _cflags=() \
    _autogen \
    _configure \
    _configure_opts=() \
    _os
  _os="$(
    uname \
      -o)"
  _autogen="${srcdir}/${_tarname}/autogen.sh"
  _configure="${srcdir}/${_tarname}/configure.sh"
  # Android sure require patches
  _cflags=(
    -Wno-implicit-function-declaration 
    -Wno-error="implicit-function-declaration"
  )
  _configure_opts+=(
    --prefix="/usr"
    --libexecdir="/usr/lib"
    --with-openssl
    gl_cv_host_operating_system="${_os}"
  )
  if [[ "${CARCH}" == "arm" || \
        "${CARCH}" == "armv8l" ]]; then
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
  fi
  export \
    CFLAGS="${CFLAGS} ${_cflags[*]}"
  cd \
    "${_tarname}"
  if [[ ! -e "${_configure}" ]]; then
    if [[ -e "${_autogen}" ]]; then
      "${_autogen}" \
        "${_configure_opts[@]}"
    else
      autoreconf \
        -i
    fi
  fi
  "${_configure}" \
    "${_configure_opts[@]}"
  make
}

check() {
  cd \
    "${_tarname}"
  make \
    check
}

package() {
  cd \
    "${_tarname}"
  # Android sure require extra steps
  make \
    DESTDIR="${pkgdir}" \
    install
}

# vim:set sw=2 sts=-1 et:
