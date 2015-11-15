pkgname=nuke-cataclysm-dda-git
pkgver=0.E.r22454.g8c0034b74e
pkgrel=2
pkgdesc="A post-apocalyptic roguelike."
url="https://github.com/CleverRaven/Cataclysm-DDA"
arch=('x86_64')
license=("CCPL:by-sa")
conflicts=('cataclysm-dda' 'cataclysm-dda-ncurses' 'cataclysm-dda-tiles' 'cataclysm-dda-git')
depends=('ncurses' 'sdl2_image' 'sdl2_ttf' 'freetype2' 'sdl2_mixer')
makedepends=('sdl2_image' 'sdl2_ttf' 'sdl2_mixer' 'freetype2' 'git' 'ccache' 'clang' 'astyle')
source=("$pkgname"::'git://github.com/CleverRaven/Cataclysm-DDA.git#branch=master')
md5sums=('SKIP')

pkgver() {
    cd $pkgname;
    echo "0.E.$(date +%Y.%m.%d)"
}

prepare() {
    cd $pkgname;
    sed -i 's/ncursesw5-config/ncursesw6-config/' Makefile;
}

build() {
    cd $pkgname;

    make PREFIX=/usr RELEASE=1 USE_XDG_DIR=1 LANGUAGE="all" CLANG=0 CCACHE=0 ZLEVELS=1 RUNTESTS=0 LINTJSON=0 ASTYLE=0 PCH=0 LOCALIZE=0
    make PREFIX=/usr RELEASE=1 USE_XDG_DIR=1 LANGUAGE="all" CLANG=0 CCACHE=0 ZLEVELS=1 RUNTESTS=0 LINTJSON=0 ASTYLE=0 PCH=0 LOCALIZE=0 TILES=1

    touch README.txt;
}

package() {
    cd $pkgname;

    make PREFIX="$pkgdir/usr" RELEASE=1 ZLEVELS=1 CLANG=0 CCACHE=0 USE_XDG_DIR=1 PCH=0 install
    make PREFIX="$pkgdir/usr" RELEASE=1 ZLEVELS=1 CLANG=0 CCACHE=0 USE_XDG_DIR=1 PCH=0 TILES=1 install

    # Icon
    install -D 'build-data/osx/AppIcon.iconset/icon_128x128.png' "$pkgdir/usr/share/icons/hicolor/128x128/apps/cataclysm-dda.png"

    # Docs
    install -d "$pkgdir/usr/share/doc/cataclysm-dda"
    cp -r doc/* "$pkgdir/usr/share/doc/cataclysm-dda"
    rm "$pkgdir/usr/share/doc/cataclysm-dda/"*.6
    install -Dm644 doc/cataclysm.6 "$pkgdir/usr/share/man/man6/cataclysm.6"
    install -Dm644 doc/cataclysm-tiles.6 "$pkgdir/usr/share/man/man6/cataclysm-tiles.6"

    # undo symlink
    rm "$pkgdir/usr/share/doc/cataclysm-dda/JSON_LOADING_ORDER.md"
    cp 'data/json/LOADING_ORDER.md' "$pkgdir/usr/share/doc/cataclysm-dda/JSON_LOADING_ORDER.md"

    # License
    install -Dm644 LICENSE.txt "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
