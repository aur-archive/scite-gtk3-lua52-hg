# Maintainer: felix <m.p.isaev@yandex.com>
pkgname=scite-gtk3-lua52-hg
pkgver=4279.5057
pkgrel=1
pkgdesc="Editor with facilities for building and running programs (Mercurial snapshot, using GTK3 and shared Lua 5.2)"
arch=(i686 x86_64)
url="http://scintilla.org/"
license=('custom:scite')
groups=()
depends=(gtk3 desktop-file-utils lua)
makedepends=(mercurial)
provides=(scite)
conflicts=(scite scite-hg)
replaces=()
backup=()
options=()
install=scite.install
source=('scintilla::hg+http://hg.code.sf.net/p/scintilla/code'
        'scite::hg+http://hg.code.sf.net/p/scintilla/scite'
        lua.patch
        scite.install)
sha1sums=('SKIP'
          'SKIP'
          '4de42d1bcc39fb4be411ace0d930034cbb783e5f'
          'ade08b9f480e97c81f5f96a1a0eb8e66142990bf')

pkgver() {
  echo "$(cd "$srcdir/scite"; hg identify -n).$(cd "$srcdir/scintilla"; hg identify -n)"
}

prepare() {
  rm -rf "$srcdir/scite/lua"

  cd "$srcdir/scite"
  patch -p1 < "$srcdir/lua.patch"

  sed -i -e '/^#define\s\s*VERSION_SCITE\s\s*/ s/".*"/"hg-'"$pkgver"'"/' src/SciTE.h

  cd gtk
  make deps
}

build() {
  cd "$srcdir/scintilla/gtk"
  make GTK3=1

  cd "$srcdir/scite/gtk"
  make GTK3=1
}

package() {
  cd "$srcdir/scite/gtk"
  make DESTDIR="$pkgdir/" install

  install -Dm644 "$srcdir/scintilla/License.txt" \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  ln -sf SciTE "$pkgdir/usr/bin/scite"
}

# vim:set ts=2 sw=2 et:
