# Maintainer: 
# Contributor: Alexander Rødseth <rodseth@gmail.com>
# Contributor: Vesa Kaihlavirta <vesa@archlinux.org>
# Contributor: Arch Haskell Team <arch-haskell@haskell.org>

pkgname=haddock
pkgver=2.9.4
pkgrel=1
pkgdesc="Tool for generating documentation for Haskell libraries"
url="http://hackage.haskell.org/package/haddock"
license=('custom:BSD3')
arch=('x86_64' 'i686')
makedepends=('alex' 'happy')
depends=('ghc<7.4' 'haskell-mtl' 'haskell-xhtml>=3000.2' 'haskell-ghc-paths') 
options=('strip')
install=$pkgname.install
source=("http://hackage.haskell.org/packages/archive/$pkgname/$pkgver/$pkgname-$pkgver.tar.gz")
sha256sums=('06f5c24e284399682d293eff7d90447ed6e525d21040f95badc9cbfb83ba85a3')

build() {
  cd "$srcdir/$pkgname-$pkgver"
  
  # These doesn't make haddock work for ghc 7.4.1
  #sed -i 's:ghc >= 7.2 && < 7.4:ghc:' haddock.cabal
  #sed -i 's:base >= 4.3 && < 4.5:base:' haddock.cabal
  #sed -i 's:#elif __GLASGOW_HASKELL__ == 703:#elif __GLASGOW_HASKELL__ == 704:' src/Haddock/InterfaceFile.hs

  runhaskell Setup configure -O -p --enable-split-objs --enable-shared --prefix=/usr \
    --docdir=/usr/share/doc/"$pkgname" --libsubdir=\$compiler/site-local/\$pkgid
  runhaskell Setup build
  runhaskell Setup haddock
  runhaskell Setup register --gen-script
  runhaskell Setup unregister --gen-script
  sed -i -r -e "s|ghc-pkg.*unregister[^ ]* |&'--force' |" unregister.sh
}

package() {
  cd "$srcdir/$pkgname-$pkgver"
  
  install -Dm744 register.sh "$pkgdir/usr/share/haskell/$pkgname/register.sh"
  install -m744 unregister.sh "$pkgdir/usr/share/haskell/$pkgname/unregister.sh"
  install -dm755 "$pkgdir/usr/share/doc/ghc/html/libraries"
  ln -s "/usr/share/doc/$pkgname/html" "$pkgdir/usr/share/doc/ghc/html/libraries/$pkgname"
  runhaskell Setup copy --destdir="$pkgdir"
  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  rm -f "$pkgdir/usr/share/doc/$pkgname/LICENSE"
  mv "$pkgdir/usr/bin/haddock" "$pkgdir/usr/bin/haddock-cabal"
}

# vim:set ts=2 sw=2 et:
