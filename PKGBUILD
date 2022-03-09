# Contributor: mar77i <mar77i at mar77i dot ch>
# Contributor: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Scytrin dai Kinthra <scytrin@gmail.com>
# Contributor: Stefan Husmann <stefan-husmann@t-online.de>
# Maintainer: Mads Mogensen <mads256h at gmail dot com>

# PKGBUILD based on st-luke-git

pkgname=st-mads256h
_pkgname=st
pkgver=0.8.5
pkgrel=2
pkgdesc='Simple virtual terminal emulator for X'
url='https://st.suckless.org/'
arch=('x86_64')
license=('MIT')
depends=('libxft')
makedepends=('ncurses' 'libxext')

# include config.h and any patches you want to have applied here
source=("https://dl.suckless.org/st/$_pkgname-$pkgver.tar.gz"
        "https://st.suckless.org/patches/background_image/st-background-image-0.8.4.diff"
        "https://st.suckless.org/patches/boxdraw/st-boxdraw_v2-0.8.3.diff"
        "https://st.suckless.org/patches/xresources/st-xresources-20200604-9ba7ecf.diff"
        "xresources-fix.diff"
        "boxdraw-fix.diff"
        "config.diff")
sha256sums=('ea6832203ed02ff74182bcb8adaa9ec454c8f989e79232cb859665e2f544ab37'
            'bdca2ab413b622c1e8708b44daa216acb37d72d252db7c1f9f26d922e1093a2e'
            'a24118148631f6670ea568a3debdd00a7cbcfa525839281888e876e7ea409658'
            '5be9b40d2b51761685f6503e92028a7858cc6571a8867b88612fce8a70514d5b'
            '2bc59fc294171a27c10c63bf418a1001bbfe643b603c23ae642ca16d522cf929'
            '28ec6417b9213d212b4e41d9ef2126a9afefb0fd8128ca3a32210ff2c708e84d'
            '69299d7070a8a313c8cbb48c01da6a7dca77f004b527e51f1781daf852f03e5b')
provides=('st')
conflicts=('st')


build() {

  # Patch xresources
  patch --follow-symlinks -i "${srcdir}/xresources-fix.diff"

  # Patch boxdraw
  patch --follow-symlinks -i "${srcdir}/boxdraw-fix.diff"

  cd "$_pkgname-$pkgver"
  patch --strip=1 --input="${srcdir}/st-background-image-0.8.4.diff"
  patch --strip=1 --input="${srcdir}/st-xresources-20200604-9ba7ecf.diff"
  patch --strip=1 --input="${srcdir}/st-boxdraw_v2-0.8.3.diff"
  patch --strip=1 --input="${srcdir}/config.diff"

  make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
  cd "$_pkgname-$pkgver"
  make PREFIX=/usr DESTDIR="${pkgdir}" install
  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
