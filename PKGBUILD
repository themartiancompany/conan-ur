# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2022, 2023, 2024, 2025  Pellegrino Prevete
#
#    All rights reserved
#    ----------------------------------------------------------------------
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General Public License
#    along with this program.  If not, see <https://www.gnu.org/licenses/>.

# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Truocolo <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
# Maintainer: Pellegrino Prevete (dvorak) <pellegrinoprevete@gmail.com>
# Maintainer: Pellegrino Prevete (dvorak) <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Maintainer: Tomislav Ivek <tomislav.ivek@gmail.com>

_evmfs_available="$( \
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
_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
_pkg=conan
pkgname=(
  "${_pkg}"
)
pkgver=1.48.1
pkgrel=1
_pkgdesc=(
  "A distributed, open source,"
  "C/C++ package manager."
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  'any'
)
url="https://${_pkg}.io"
license=(
  'MIT'
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
  "${_py}-pyjwt>=1.4.0"
  "${_py}-requests>=2.25"
  "${_py}-urllib3>=1.26.6"
  "${_py}-colorama>=0.3.3"
  "${_py}-yaml>=3.11"
  "${_py}-patch-ng>=1.17.4"
  "${_py}-fasteners>=0.14.1"
  "${_py}-six>=1.10.0"
  "${_py}-node-semver>=0.6.1"
  "${_py}-distro>=1.0.2"
  "${_py}-pygments>=2.0"
  "${_py}-tqdm>=4.28.1"
  "${_py}-jinja>=3.0"
  "${_py}-dateutil>=2.7.0"
  "${_py}-bottle>=0.12.8"
  "${_py}-pluginbase>=0.5"
)
makedepends=(
  "${_py}"
  "${_py}-setuptools"
  'patch'
)
provides=(
  "${_py}-${_pkg}=${pkgver}"
)
conflicts=(
  "${_py}-${_pkg}"
)
_http="https://github.com"
_ns="${_pkg}-io"
_url="${_http}/${_ns}/${_pkg}"
_tarname="${_pkg}-${pkgver}"
_evmfs_network="17000"
_evmfs_fs="0x151920938488F193735e83e052368cD41F9d9362"
# Dvorak namespace
_evmfs_ns="0x87003Bd6C074C713783df04f36517451fF34CBEf"
_evmfs_dir="evmfs://${_evmfs_network}/${_evmfs_fs}/${_evmfs_ns}"
_archive_sum='9e43869d6b016aedd7f0a4b22b4c068b8af5c3569ed7a66c63225c388d15cce9'
_evmfs_uri="${_evmfs_dir}/${_archive_sum}"
_http_uri="${_url}/archive/${pkgver}.tar.gz"
if [[ "${_evmfs}" == "true" ]]; then
  makedepends+=(
    "evmfs"
  )
  _src="${_tarname}.tar.gz::${_evmfs_uri}"
elif [[ "${_evmfs}" == "false" ]]; then
  _src="${_tarname}.tar.gz::${_http_uri}"
fi
source=(
  "${_src}"
  "arch-reqs.patch"
)
sha256sums=(
  "${_archive_sum}"
  'c8a8c67ee6847c077c4e361a633fb4772c0d30bd448a334e43ba786ce408b419'
)

prepare() {
  cd \
    "${_tarname}"
  patch \
    -Np1 \
    -i \
    "${srcdir}/arch-reqs.patch"
}

build() {
  cd \
    "${_tarname}"
  "${_py}" \
    "setup.py" \
      build
}

package() {
  cd \
    "${_tarname}"
  "${_py}" \
    "setup.py" \
      install \
        --optimize=1 \
	--root="${pkgdir}"
  install \
    -vdm755 \
    "${pkgdir}/usr/share/licenses/${pkgname}"
  install \
    -vm644 \
    "LICENSE.md" \
    "${pkgdir}/usr/share/licenses/${pkgname}/"
  install \
    -vdm755 \
    "${pkgdir}/usr/share/doc/${_pkg}"
  install \
    -vm644 \
    "contributors.txt" \
    "${pkgdir}/usr/share/doc/${_pkg}/"
}
