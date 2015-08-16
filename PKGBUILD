# Maintainer: Gustavo Alvarez <sl1pkn07@gmail.com>

pkgname=libdvdcss-svn
pkgver=256
pkgrel=2
pkgdesc="A portable abstraction library for DVD decryption. (SVN version)"
arch=('i686' 'x86_64')
license=('GPL')
url="http://www.videolan.org/libdvdcss"
depends=('glibc')
makedepends=('svn' 'texlive-latexextra' 'doxygen')
options=('!libtool')
provides=('libdvdcss')
conflicts=('libdvdcss')

_svntrunk="svn://svn.videolan.org/libdvdcss/trunk"
_svnmod="libdvdcss"

build() {
  cd "${srcdir}"

  msg "Connecting to SVN server...."

  if [ -d "${srcdir}/${_svnmod}" ] ; then
      cd "${_svnmod}" && svn update
  else
      svn co "${_svntrunk}" "${_svnmod}"
  fi

  msg "SVN checkout done or server timeout"
  msg "Starting make..."

  rm -fr "${srcdir}/${_svnmod}-build"
  cp -R "${srcdir}/${_svnmod}" "${srcdir}/${_svnmod}-build"
  cd  "${srcdir}/${_svnmod}-build"

  # Fix for automake 1.13.x
  sed -e 's|AM_CONFIG_HEADER(config.h)|AC_CONFIG_HEADERS([config.h])|' -e '5a AC_CONFIG_MACRO_DIR([m4])' -i configure.ac
  sed 's|for v in 12|for v in 13 12|' -i bootstrap
  sed '2a ACLOCAL_AMFLAGS = -I m4' -i Makefile.am

  # Fix include path in libdvdcss.pc
  sed 's| -I${includedir}/@PACKAGE@||g' -i src/libdvdcss.pc.in
  ./bootstrap
  ./configure --prefix=/usr
  make
}

package() {
  cd "${srcdir}/${_svnmod}-build"
  make DESTDIR="${pkgdir}" install
  install -d "${pkgdir}/usr/share/doc/libdvdcss"
  install -m644 doc/html/* "${pkgdir}/usr/share/doc/libdvdcss/"
}
