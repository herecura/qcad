# $Id$
# Maintainer: BlackIkeEagle <ike DOT devolder AT gmail DOT com>
# Contributor: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Stefan Husmann <stefan-husmann@t-online.de>
# Contributor: Giovanni Scafora <linuxmania@gmail.com>
# Contributor: Daniel J Griffiths <ghost1227@archlinux.us>

pkgname=qcad
pkgver=3.19.2.0
pkgrel=1
pkgdesc='A 2D CAD package based upon Qt'
arch=('x86_64')
url="http://www.qcad.org"
license=('GPL3')
depends=('qt5-script' 'qt5-svg' 'gcc-libs' 'qt5-xmlpatterns')
makedepends=('qt5-tools' 'glu')
source=("$pkgname-$pkgver.tar.gz::https://github.com/qcad/qcad/archive/v${pkgver}.tar.gz")
sha512sums=('70055910c341751b759fc6f58ba07a299c19ad4e7072af2c6e5ae2967a05790dc3f9b8d81c6affc008179bd42bb64bf3a3ef8a79c8c578bf53e4b326f505bf9b')

prepare() {
  rm *.tar.gz
  cd qcad-$pkgver
  sed -e 's|$${QT_VERSION}|5.5.0|g' \
      -i src/3rdparty/3rdparty.pro # Don't require specific Qt version
}

build() {
  cd qcad-$pkgver
  qmake-qt5 qcad.pro
  make
}

package() {
  cd qcad-$pkgver

  # remove project files
  find . \( -name '*.pri' -or -name '.pro' -or -name '*.ts' \) -delete
  find . \( -name 'Makefile' -name '.gitignore' \) -delete

  install -dm755 "$pkgdir"/usr/lib/qcad
  cp -r examples fonts libraries linetypes patterns plugins scripts ts \
      "$pkgdir"/usr/lib/qcad
  cp release/* "$pkgdir"/usr/lib/qcad

  install -m755 readme.txt "$pkgdir"/usr/lib/qcad/readme.txt

  # qt
  for sofiles in /usr/lib/qt/plugins/imageformats/*.so
  do
    ln -sf ${sofiles} "$pkgdir"/usr/lib/qcad/plugins/imageformats/${sofiles##/*/}
  done
  for sofiles in /usr/lib/qt/plugins/sqldrivers/*.so
  do
    ln -sf ${sofiles} "$pkgdir"/usr/lib/qcad/plugins/sqldrivers/${sofiles##/*/}
  done

  install -Dm644 scripts/qcad_icon.png "$pkgdir"/usr/share/pixmaps/qcad_icon.png
  install -Dm644 qcad.desktop "$pkgdir"/usr/share/applications/qcad.desktop

  install -dm0755 "$pkgdir"/usr/bin
  echo -e '#!/bin/sh\nLD_LIBRARY_PATH=${LD_LIBRARY_PATH:+$LD_LIBRARY_PATH:}"/usr/lib/qcad" exec /usr/lib/qcad/qcad-bin "$@"' >"$pkgdir"/usr/bin/qcad

  chmod 0755 "$pkgdir"/usr/bin/qcad
}
