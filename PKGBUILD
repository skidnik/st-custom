# Maintainer skidnik < skidnik at gmail dot com >

pkgname=st-custom
pkgver=0.8.1
pkgrel=1
pkgdesc='A simple virtual terminal emulator for X. This build uses custom patches and config.h' 
arch=('i686' 'x86_64')
license=('MIT')
depends=('libxft' 'libxext' 'xorg-fonts-misc')
makedepends=('ncurses')
conflicts=('st')
provides=('st')
url="http://st.suckless.org"
source=(http://dl.suckless.org/st/st-$pkgver.tar.gz)
sha256sums=('c4fb0fe2b8d2d3bd5e72763e80a8ae05b7d44dbac8f8e3bb18ef0161c7266926')
# edit below if you want a different directory for patches and config.h
# udir=$HOME/.config/$pkgname-$pkgver
# udir=$srcdir/patches
udir=/usr/src/$pkgname-$pkgver
prepare() {
  cd $srcdir/st-$pkgver
  if [ -f $udir/patches/* ] 
  then
  	for patch in $udir/patches/* 
  	do
   	    patch -i $patch
	    echo "Applying $patch"
  	done
  fi
  if [ -f $udir/config.h ]
  then
	  cp $udir/config.h .
	  echo "Using saved config.h"
  else
  	  cp config.def.h config.h
	  echo "Using config.def.h"
  fi
  # skip terminfo which conflicts with nsurses
  sed -i '/\@tic /d' Makefile
  # set font to Droid Sans Mono
  # sed -i 's/Liberation\sMono/Droid Sans Mono/' config.h
}

build() {
  cd $srcdir/st-$pkgver
  make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
  cd $srcdir/st-$pkgver
  make PREFIX=/usr DESTDIR="$pkgdir" TERMINFO="$pkgdir/usr/share/terminfo" install
  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/st/LICENSE"
  install -Dm644 README "$pkgdir/usr/share/doc/st/README"
  install -dm755 "$pkgdir$udir/patches"
  install -m644 config.h "$pkgdir$udir/config.h"
  tput setaf 2; echo "==>This package attempts to apply patches from $udir/patches/ directory and uses custom config.h from $udir/, if none provided, it builds the default st. Put the desired files there and rebuild. Note that some patches are applied to config.h, so edit it after building a patched version."; tput sgr0; 
}
