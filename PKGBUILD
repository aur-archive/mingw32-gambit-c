# Maintainer: gravicappa <ramil@gmx.co.uk>
# Contributor: Philipp Brüschweiler <blei42@gmail.com>
# Contributor: Sebastian A. Liem <sebastian@liem.se>
# Contributor: Stefan Husmann <stefan-husmann@t-online.de>
pkgname=mingw32-gambit-c
_pkgname=gambc
pkgver=4.6.4
_pkgver="$(echo $pkgver | sed 's/\./_/g')"
pkgrel=2
pkgdesc="A Scheme interpreter and a optimized compiler which generates \
portable C code (mingw32)"
url="http://www.iro.umontreal.ca/~gambit/"
depends=('sh' 'mingw32-gcc' 'mingw32-w32api')
makedepends=('texinfo')
arch=('i686' 'x86_64')
license=('LGPL' 'APACHE')
options=(!libtool)
source=(http://www.iro.umontreal.ca/~gambit/download/gambit/v${pkgver%??}/source/$_pkgname-v$_pkgver.tgz)
md5sums=('f4a65f834b36b7ffbd0292021889a8e3')

build() {
  cd $srcdir/$_pkgname-v${pkgver//\./_}
  windir=
  crossdir=

  unset LDFLAGS
  unset CFLAGS
  export CFLAGS='-O2'

  ./configure \
    --prefix=/usr/i486-mingw32/$crossdir \
    --infodir=/usr/i486-mingw32/$crossdir/share/info \
    --libdir=/usr/i486-mingw32/$crossdir/lib \
    --includedir=/usr/i486-mingw32/$crossdir/include \
    --docdir=/usr/i486-mingw32/$crossdir/share/doc/$pkgname \
    --bindir=/usr/i486-mingw32/$crossdir/bin \
    --enable-gcc-opts --enable-single-host \
    --without-x || return 1

  sed -i '/^GAMBCLIB_DEFS/s/$/ -D___DOUBLE_QUOTE_PROGRAM_ARGS/' lib/makefile \
      makefile

  (make CFLAGS='-D___DOUBLE_QUOTE_PROGRAM_ARGS' && make doc) || return 1

  sed -i \
    '/^C_COMPILER=/ s/"gcc"/"i486-mingw32-gcc"/
     /^C_PREPROC=/ {
       s/"gcc -E"/"i486-mingw32-gcc -E"/
     }
     /^LIBS=/ cLIBS="-lm "' \
    bin/gambc-cc || return 1

  # Remove if you want to specify include and lib dirs explicitely
  #sed -i \
  # '/^C_PREPROC=/ {
  #   aGAMBCDIR_INCLUDE=\${CUSTOM_GAMBCDIR_INCLUDE:-/usr/i486-mingw32/include}
  #   aGAMBCDIR_LIB=\${CUSTOM_GAMBCDIR_LIB:-/usr/i486-mingw32/lib}
  # }' \
  # bin/gambc-cc.bat

  make DESTDIR=$pkgdir install || return 1
  mv $pkgdir/usr/i486-mingw32/$crossdir/bin $pkgdir/ || return 1
  rm -rf $pkgdir/usr/i486-mingw32 || return 1

  make clean distclean || return 1
  ./configure \
    --host=i486-mingw32 \
    --prefix=/usr/i486-mingw32/$windir \
    --infodir=/usr/i486-mingw32/$windir/share/info \
    --libdir=/usr/i486-mingw32/$windir/lib \
    --includedir=/usr/i486-mingw32/$windir/include \
    --docdir=/usr/i486-mingw32/$windir/share/doc/$pkgname \
    --bindir=/usr/i486-mingw32/$windir/bin \
    --enable-gcc-opts --enable-single-host \
    --without-x || return 1

  sed -i '/^#define socklen_t/d' include/config.h || return 1

  sed -i '/^GAMBCLIB_DEFS/s/$/ -D___DOUBLE_QUOTE_PROGRAM_ARGS/' lib/makefile \
      makefile

  (make && make doc) || return 1

  make DESTDIR=$pkgdir install || return 1
  #rm -rf $pkgdir/usr/i486-mingw32/$windir/bin || return 1
  cp -r $pkgdir/bin $pkgdir/usr/i486-mingw32/ || return 1
  rm -rf $pkgdir/bin || return 1
}
