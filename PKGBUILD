# Maintainer: BlackIkeEagle <ike DOT devolder AT gmail DOT com>
# Contributor: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Stefan Husmann <stefan-husmann@t-online.de>
# Contributor: Giovanni Scafora <linuxmania@gmail.com>
# Contributor: Daniel J Griffiths <ghost1227@archlinux.us>

pkgname=qcad
pkgver=3.24.3.4
pkgrel=1
pkgdesc='A 2D CAD package based upon Qt'
arch=('x86_64')
url="https://www.qcad.org"
license=('GPL3')
depends=('qt5-script' 'qt5-svg' 'gcc-libs' 'qt5-xmlpatterns' 'qt5-tools')
makedepends=('glu')
source=("$pkgname-$pkgver.tar.gz::https://github.com/qcad/qcad/archive/v${pkgver}.tar.gz")
sha512sums=('bd690e4cde854b7348f374ecb8b3f1f3a98452aeee789fffb67a7c8d6803d583e35e2e32e410dd96b98d5b19f91c042e5e4b378762260768057e293aec5ef6a2')

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
  cp -r examples fonts libraries linetypes patterns scripts themes ts \
      "$pkgdir"/usr/lib/qcad
  cp release/{*.so,qcad-bin} "$pkgdir"/usr/lib/qcad

  # install man
  install -Dm644 qcad.1 "$pkgdir"/usr/share/man/man1/qcad.1

  # readme
  install -Dm644 readme.txt "$pkgdir"/usr/lib/qcad/readme.txt

  # qt
  cp -r plugins platforminputcontexts platforms xcbglintegrations \
      "$pkgdir"/usr/lib/qcad
  #install -dm755 "$pkgdir"/usr/lib/qcad/plugins/{designer,imageformats,printsupport,sqldrivers}
  #for sofiles in /usr/lib/qt/plugins/designer/*.so
  #do
    #ln -sf ${sofiles} "$pkgdir"/usr/lib/qcad/plugins/designer/${sofiles##/*/}
  #done
  #for sofiles in /usr/lib/qt/plugins/imageformats/*.so
  #do
    #ln -sf ${sofiles} "$pkgdir"/usr/lib/qcad/plugins/imageformats/${sofiles##/*/}
  #done
  #for sofiles in /usr/lib/qt/plugins/printsupport/*.so
  #do
    #ln -sf ${sofiles} "$pkgdir"/usr/lib/qcad/plugins/printsupport/${sofiles##/*/}
  #done
  #for sofiles in /usr/lib/qt/plugins/sqldrivers/*.so
  #do
    #ln -sf ${sofiles} "$pkgdir"/usr/lib/qcad/plugins/sqldrivers/${sofiles##/*/}
  #done
  #install -dm755 "$pkgdir"/usr/lib/qcad/{platforminputcontexts,platforms,xcbglintegrations}
  #for sofiles in /usr/lib/qt/plugins/platforminputcontexts/*.so
  #do
    #ln -sf ${sofiles} "$pkgdir"/usr/lib/qcad/platforminputcontexts/${sofiles##/*/}
  #done
  #for sofiles in /usr/lib/qt/plugins/platforms/*.so
  #do
    #ln -sf ${sofiles} "$pkgdir"/usr/lib/qcad/platforms/${sofiles##/*/}
  #done
  #for sofiles in /usr/lib/qt/plugins/xcbglintegrations/*.so
  #do
    #ln -sf ${sofiles} "$pkgdir"/usr/lib/qcad/xcbglintegrations/${sofiles##/*/}
  #done

  install -Dm644 scripts/qcad_icon.png "$pkgdir"/usr/share/pixmaps/qcad_icon.png
  install -Dm644 qcad.desktop "$pkgdir"/usr/share/applications/qcad.desktop

  install -dm755 "$pkgdir"/usr/bin
  echo -e '#!/bin/sh\nLD_LIBRARY_PATH=${LD_LIBRARY_PATH:+$LD_LIBRARY_PATH:}"/usr/lib/qcad" exec /usr/lib/qcad/qcad-bin "$@"' >"$pkgdir"/usr/bin/qcad

  chmod 0755 "$pkgdir"/usr/bin/qcad
}
