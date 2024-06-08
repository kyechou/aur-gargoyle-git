# Maintainer: SpaghettiCat <d_the_m101 at yahoo dot co dot uk>
# Based on "gargoyle" package maintained by:
#     Beej <beej@beej.us>
#     Michael Smith <michael at diglumi dot com>
#     Marcin Skory <armitage at q84fh dot net>
#     with Contribution of Eric Forgeot < http://ifiction.free.fr >

pkgname=gargoyle-git
pkgver=2022.1.r691.g46c97ec5
pkgrel=1
pkgdesc="Interactive Fiction multi-interpreter that supports all major IF formats (development version)"
arch=('i686' 'x86_64' 'armv6h' 'armv7h' 'aarch64')
url="https://github.com/garglk/garglk"
license=('GPL2' 'GPL3' 'custom:BSD-2-Clause' 'custom:BSD-3-Clause' 'Artistic2.0'
         'MIT' 'custom:OFL-1.1')
depends=('sdl2_mixer' 'sdl2' 'freetype2' 'qt6-base' 'fontconfig' 'libjpeg'
         'libpng' 'zlib' 'hicolor-icon-theme')
makedepends=('cmake' 'pkgconfig' 'desktop-file-utils' 'git' 'fmt')
optdepends=('speech-dispatcher: text-to-speech support')
provides=('gargoyle')
conflicts=('gargoyle-mod' 'gargoyle')
replaces=('gargoyle-mod' 'gargoyle')
backup=('etc/garglk.ini')
# groups=(inform)
install=$pkgname.install
source=("$pkgname"::'git+https://github.com/garglk/garglk.git')
sha512sums=('SKIP')

pkgver() {
    cd "$srcdir/$pkgname"
    if git describe --long --tags >/dev/null 2>&1; then
        git describe --long --tags | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
    else
        printf 'r%s.%s' "$(git rev-list --count HEAD)" "$(git describe --always)"
    fi
}

build() {
    cd "$srcdir/$pkgname"

    # Extract the license for Git from the readme
    sed -n '/Copyright (c)/,/DEALINGS IN THE SOFTWARE\./p' \
        terps/git/README.txt > terps/git/LICENSE

    cmake -S . -B build \
        -DCMAKE_BUILD_TYPE=Release \
        -DWITH_QT6=true \
        -DWITH_TTS=DYNAMIC \
        -DCMAKE_INSTALL_LIBEXECDIR=lib \
        -DCMAKE_INSTALL_PREFIX=/usr
    cmake --build build
}

package() {
    cd "$srcdir/$pkgname"
    DESTDIR="$pkgdir" cmake --install build

    # Install default config
    install -Dm644 "$srcdir/$pkgname/garglk/garglk.ini" -t "$pkgdir/etc/"

    # Install licenses
    local license_dir="$pkgdir/usr/share/licenses/$pkgname"
    install -Dm644 "$srcdir/$pkgname/License.txt" "$license_dir/LICENSE"
    install -Dm644 "$srcdir/$pkgname/licenses/BSD-2-Clause.txt" "$license_dir/BSD-2-Clause.txt"
    install -Dm644 "$srcdir/$pkgname/licenses/Go Mono.txt" "$license_dir/BSD-3-Clause-Go-Mono.txt"
    install -Dm644 "$srcdir/$pkgname/terps/advsys/LICENSE" "$license_dir/BSD-3-Clause-AdvSys.txt"
    install -Dm644 "$srcdir/$pkgname/terps/git/LICENSE" "$license_dir/MIT-Git.txt"
    install -Dm644 "$srcdir/$pkgname/terps/glulxe/LICENSE" "$license_dir/MIT-Glulxe.txt"
    install -Dm644 "$srcdir/$pkgname/licenses/Charis SIL.txt" "$license_dir/OFL-1.1.txt"
}

# vim: set ts=4 sw=4 et :
