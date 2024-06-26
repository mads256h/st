# Contributor: mar77i <mar77i at mar77i dot ch>
# Contributor: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Scytrin dai Kinthra <scytrin@gmail.com>
# Contributor: Stefan Husmann <stefan-husmann@t-online.de>
# Maintainer: Mads Mogensen <mads256h at gmail dot com>

# PKGBUILD based on st-luke-git

pkgname=st-mads256h
_pkgname=st
pkgver=0.9.1
pkgrel=6
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
        "https://st.suckless.org/patches/xresources-with-reload-signal/st-xresources-signal-reloading-20220407-ef05519.diff"
        "https://st.suckless.org/patches/dynamic-cursor-color/st-dynamic-cursor-color-0.9.diff"
        "https://st.suckless.org/patches/undercurl/st-undercurl-0.9-20240103.diff"
        "xresources-fix.diff"
        "config.diff"
        "update-bg-on-reload.diff"
        "rename-terminal.diff"
        "undercurl-smaller-linewidth.diff")
sha256sums=('16f43b9433ade9d70d6085c31f9fd99f2835eaade31221020f22143035dfc0d2'
            '1268ffd79bdf5c01439f4bec76bddb6f8f92d331fc647ce2f67143bf036e45a5'
            '046452606fceb62abc8f068d891c5a5ea3d8d2b8a713d053b568f97100181081'
            '89e447a2f7b14db1fa33819ed6ea70096f7e3b68cba54b3e159b8db2b50e4bc2'
            'aaacc6dc203b66b69793bc748d9eaedd0f1f8c4417f64a2f1d72d6a2c098fec2'
            'ad44140d10ce37cae4f42a1159eac3626e6b48d061b309455b2abd93a9aa5b7c'
            '9675409eb7e809a993b9f5b50356159a66cab320eb28f03985b7469ac1f4714b'
            '69299d7070a8a313c8cbb48c01da6a7dca77f004b527e51f1781daf852f03e5b'
            '26bf44a763c167d376ad70974ed5b6652182e8d68e57cba7afa10de0ae139ac3'
            'f5e32bcd6fc4ae9bc7c043632ee469da795f08c72c31a9a8130d3c0e47184d36'
            '2ea5ec4245423b46975e55e3a9b2496aa92a6d773dfa5d477488b98f16382260')
provides=('st')
conflicts=('st')


build() {

  # Patch xresources
  patch --follow-symlinks -i "${srcdir}/xresources-fix.diff"

  # Patch undercurl
  patch --follow-symlinks -i "${srcdir}/undercurl-smaller-linewidth.diff"

  cd "$_pkgname-$pkgver"
  patch --strip=1 --input="${srcdir}/st-background-image-0.8.5.diff"
  patch --strip=1 --input="${srcdir}/st-xresources-signal-reloading-20220407-ef05519.diff"
  patch --strip=1 --input="${srcdir}/st-boxdraw_v2-0.8.5.diff"
  patch --strip=1 --input="${srcdir}/st-dynamic-cursor-color-0.9.diff"
  patch --strip=1 --input="${srcdir}/st-undercurl-0.9-20240103.diff"
  patch --strip=1 --input="${srcdir}/config.diff"
  patch --strip=1 --input="${srcdir}/update-bg-on-reload.diff"
  patch --strip=1 --input="${srcdir}/rename-terminal.diff"

  # Patch makefile to make terminfo in the pkgdir
  # Also rename the terminfo to st-mads256h
  sed -i "/.*tic -sx st.info$/d" "Makefile"
  sed 's/st\([^t].*\)/st-mads256h\1/g' st.info > st-mads256h.info
  TERMINFO="${srcdir}/terminfo" tic -sx st-mads256h.info
  make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
  cd "$_pkgname-$pkgver"
  mkdir -p "${pkgdir}/usr/share/"
  cp -a "${srcdir}/terminfo" "${pkgdir}/usr/share/"
  make PREFIX=/usr DESTDIR="${pkgdir}" install
  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
