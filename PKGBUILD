# Maintainer: Qhor Vertoe <vertoe at qhor dot net>

# Contributors by way of cgminer package in arch repos
# Contributor: roobre <roobre@roobre.es>
# Contributor: Felix Yan <felixonmars@gmail.com>
# Contributor: monson <holymonson@gmail.com>
# Contributor: Thomas Dziedzic < gostrc at gmail >
# Contributor: David Manouchehri <david@davidmanouchehri.com>

# Contributor by way of cgminer-git in AUR
# Contributor: Atterratio <atterratio at gmail>

# Contributor oiriginal cgminer-git in AUR
# Contributor: deusstultus <deusstultus@gmail.com>

pkgname=cgminer-gridseed-git
provides=('cgminer')
_gitname=cgminer-gc3355
epoch=1
pkgver=18.1634d88
pkgver() {
  cd "$srcdir/$_gitname"
  echo $(git rev-list --count HEAD).$(git rev-parse --short HEAD)
}
pkgrel=2
pkgdesc="Multi-threaded multi-pool Gridseed miner for SHA256 coins."
url='https://github.com/girnyau/cgminer-gc3355'
license=('GPL3')
arch=('any')
depends=('curl' 'libusbx' 'jansson')
conflicts=('cgminer' 'cgminer-git' 'cgminer-git-adl')
makedepends=('git' 'patch' 'autoconf' 'automake' 'binutils' 'pkg-config' 'gcc' 'libtool' 'make')
optdepends=('ncurses: For ncurses formatted screen output')
source=("git+https://github.com/girnyau/cgminer-gc3355.git"
        "system-jansson-libusb.patch"
        "$_gitname.conf.d::https://projects.archlinux.org/svntogit/community.git/plain/trunk/cgminer.conf.d?h=packages/cgminer"
        "$_gitname.service::https://projects.archlinux.org/svntogit/community.git/plain/trunk/cgminer.service?h=packages/cgminer")
sha512sums=('SKIP'
            '4b0556c7e584b49f94591d62fb7833ae80182468ff53571da15b710f7e054f4b46359b43f8d92d102097822dd9f2d9059dd73aad81849a03308b0d55a8f2fb12'
            '99c38bc395848f9712ce172343d31f5c60f5d8ac1cfe2f48df8f3ec6c488fc275763a79c5ef36b99f32faa465b5a65284b38e8a63ef9b144075ee13971313b41'
            '3317b60c6b1f14c47d8ee636113ef40a4023ab14054129de80a37947b381fd2b647a7053f4e1bb639efa225a514e862fa531908714c34040dda2d6221dde7f5f')

backup=("etc/conf.d/$_gitname" "etc/$_gitname.conf")
[ "$CARCH" == "x86_64" ] && makedepends+=('yasm')

build() {
  cd "$srcdir/$_gitname"

  # We have latest jansson and libusb - just use them
  #patch -Np1 -i ../system-jansson-libusb.patch
  #rm -r compat

  #NOCONFIGURE=1 ./autogen.sh

  # Here you may want to use custom CFLAGS
  #export CFLAGS="-O2 -march=native -mtune=native -msse2"

  ./configure --enable-scrypt --enable-gridseed
  make -j $(nproc)
}

package() {
  cd "$srcdir/$_gitname"

  make DESTDIR="$pkgdir" install

  install -Dm644 "$srcdir"/$_gitname.service "$pkgdir"/usr/lib/systemd/system/$_gitname.service
  install -Dm644 "$srcdir"/$_gitname.conf.d "$pkgdir"/etc/conf.d/$_gitname
  sed 's#/usr/local/bin#/usr/bin#g' example.conf > $_gitname.conf
  install -Dm644 $_gitname.conf "$pkgdir"/etc/$_gitname.conf
}
