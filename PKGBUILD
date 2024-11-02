# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Daniel M. Capella <polyzen@archlinux.org>

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
_pkg=sphinxcontrib-jsmath
_Pkg="sphinxcontrib_jsmath"
_pkgname="sphinx-doc-jsmath"
pkgname="${_py}-${_pkg}"
pkgver=1.0.1
pkgrel=17
pkgdesc='Sphinx extension which renders display math in HTML via JavaScript'
arch=(
  'any'
)
_http="https://github.com"
_ns="sphinx-doc"
url="${_http}/${_ns}/${_pkg}"
license=(
  'BSD'
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
)

makedepends=(
  "${_py}-build"
  "${_py}-installer"
  "${_py}-setuptools"
  "${_py}-wheel"
)
checkdepends=(
  "${_py}-pytest"
  "${_py}-sphinx"
)
_pypi="https://files.pythonhosted.org/packages/source"
source=(
  "${_pypi}/${_pkg::1}/${_pkg}/${_pkg}-${pkgver}.tar.gz"
  "jsmath-read_text.patch::${url}/commit/3297b27177ab4862d1b2408a2db66235397fe212.patch"
)
sha256sums=(
  'a9925e4a4587247ed2191a22df5f6970656cb8ca2bd6284309578f2153e0c4b8'
  '697764efeaa1a6ef234d516a4a6b3fde0c77197a89eaae3246daf8b3ad1f12df'
)
b2sums=(
  '055ff298e11678d7d30975e4bef509ece0128be30ca0c5fd2be1323c2eb4fe92f861826ea5ddfcbd2d3e3a80535b374d2b1a13446c2604f3e448d5a8982b9881'
  '028e21e345de13f86e11bd5e7b5bd80c1add25b50321c983db3c61ebf5de9c776750fc763584abab0485a6e5e470b19dcbdbd1a068be35ddd18e4b4285fe569a'
)

prepare() {
  cd \
    "${_pkg}-${pkgver}"
  # https://github.com/sphinx-doc/sphinxcontrib-jsmath/pull/10
  patch \
    --forward \
    --strip=1 \
    --input=../jsmath-read_text.patch
}

build() {
  cd "${_pkg}-${pkgver}"
  "${_py}" \
    -m \
      build \
    --wheel \
    --skip-dependency-check \
    --no-isolation
}

check() {
  cd "${_pkg}-${pkgver}"
  pytest
}

package() {
  local \
    _site_packages
  _site_packages=$( \
    "${_py}" \
      -c \
        "import site; print(site.getsitepackages()[0])")
  cd \
    "${_pkg}-${pkgver}"
  "${_py}" \
    -m \
      installer \
    --destdir="${pkgdir}" \
    dist/*.whl
  install \
    -d \
    "${pkgdir}/usr/share/licenses/${pkgname}"
  ln \
    -s \
    "${_site_packages}/${_Pkg}-${pkgver}.dist-info/LICENSE" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
