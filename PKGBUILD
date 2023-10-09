# Contributor: mar77i <mar77i at mar77i dot ch>
# Contributor: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Scytrin dai Kinthra <scytrin@gmail.com>
# Contributor: Stefan Husmann <stefan-husmann@t-online.de>
# Maintainer: Mads Mogensen <mads256h at gmail dot com>

# PKGBUILD based on st-luke-git

pkgname=st-mads256h
_pkgname=st
pkgver=0.9
pkgrel=2
pkgdesc='Simple virtual terminal emulator for X'
url='https://st.suckless.org/'
arch=('x86_64')
license=('MIT')
depends=('libxft')
makedepends=('ncurses' 'libxext')

# include config.h and any patches you want to have applied here
source=("https://dl.suckless.org/st/$_pkgname-$pkgver.tar.gz"
        "https://st.suckless.org/patches/background_image/st-background-image-0.8.5.diff"
        "https://st.suckless.org/patches/boxdraw/st-boxdraw_v2-0.8.5.diff"
        "https://st.suckless.org/patches/xresources/st-xresources-20200604-9ba7ecf.diff"
        "xresources-fix.diff"
        "config.diff")
sha256sums=('f36359799734eae785becb374063f0be833cf22f88b4f169cd251b99324e08e7'
            '1268ffd79bdf5c01439f4bec76bddb6f8f92d331fc647ce2f67143bf036e45a5'
            '046452606fceb62abc8f068d891c5a5ea3d8d2b8a713d053b568f97100181081'
            '5be9b40d2b51761685f6503e92028a7858cc6571a8867b88612fce8a70514d5b'
            '2bc59fc294171a27c10c63bf418a1001bbfe643b603c23ae642ca16d522cf929'
            '69299d7070a8a313c8cbb48c01da6a7dca77f004b527e51f1781daf852f03e5b')
provides=('st')
conflicts=('st')


build() {

  # Patch xresources
  patch --follow-symlinks -i "${srcdir}/xresources-fix.diff"

  cd "$_pkgname-$pkgver"
  patch --strip=1 --input="${srcdir}/st-background-image-0.8.5.diff"
  patch --strip=1 --input="${srcdir}/st-xresources-20200604-9ba7ecf.diff"
  patch --strip=1 --input="${srcdir}/st-boxdraw_v2-0.8.5.diff"
  patch --strip=1 --input="${srcdir}/config.diff"

  # Patch makefile to make terminfo in the pkgdir
  sed -i "/.*tic -sx st.info$/d" "Makefile"
  TERMINFO="${srcdir}/terminfo" tic -sx st.info

  make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
  cd "$_pkgname-$pkgver"
  mkdir -p "${pkgdir}/usr/share/"
  cp -ra "${srcdir}/terminfo" "${pkgdir}/usr/share/"
  make PREFIX=/usr DESTDIR="${pkgdir}" install
  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
