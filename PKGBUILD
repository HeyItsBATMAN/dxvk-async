pkgbase=dxvk-git
pkgname=('dxvk-win64-git' 'dxvk-win32-git' 'dxvk-git')
pkgver=0.94.r6.g4e22e4b
pkgrel=1
pkgdesc="A Vulkan-based compatibility layer for Direct3D 10/11 which allows running 3D applications on Linux using Wine."
arch=('x86_64')
url="https://github.com/doitsujin/dxvk"
license=('zlib/libpng')
depends=('vulkan-icd-loader' 'wine>=4.0rc1' 'winetricks')
makedepends=('ninja' 'meson>=0.43' 'glslang' 'git' 'wine')

source $PWD/customization.cfg

source=($pkgbase::"git+https://github.com/doitsujin/dxvk.git"
    "setup_dxvk_aur.verb"
    "https://raw.githubusercontent.com/jomihaka/dxvk-poe-hack/master/pipeline.patch"
)

md5sums=('SKIP'
         '63d0a0ac0927d01d256bf7d781b5111b'
         '75eaa62316afb29d08e65114f11d1557')

pkgver() {
    cd dxvk
    git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g;s/v//g'
}

prepare() {
	cd 'dxvk-git'

	# Checkout different commit
	if [ $_use_dxvk == "true" ]; then
		git reset --hard "${_dxvk_commit}"
	fi

	# Apply patch
	patch -Np1 < ../'pipeline.patch'
	
	cd ..
}

build() {
    meson dxvk "build/x64" \
        --cross-file dxvk/build-wine64.txt \
        --prefix "/usr/share/dxvk/x64" \
        --bindir "" --libdir "" \
        --buildtype "release" \
        --strip \
        -D enable_tests=false
    ninja -C "build/x64"

    meson dxvk "build/x32" \
        --cross-file dxvk/build-wine32.txt \
        --prefix "/usr/share/dxvk/x32" \
        --bindir "" --libdir "" \
        --buildtype "release" \
        --strip \
        -D enable_tests=false
    ninja -C "build/x32"
}

_package_dxvk() {
    DESTDIR="$pkgdir" ninja -C "build/x$1" install
    mkdir -p "$pkgdir/usr/bin"
    cat setup_dxvk_aur.verb | sed s/"DXVK_ARCH=64"/"DXVK_ARCH=$1"/g > "$pkgdir/usr/share/dxvk/x$1/setup_dxvk_aur.verb"
    install -Dm 644 setup_dxvk_aur.verb "$pkgdir/usr/share/dxvk/x$1/setup_dxvk_aur.verb"
    sed "s/_ARCH/$1/g" -i "$pkgdir/usr/share/dxvk/x$1/setup_dxvk_aur.verb"
    echo "#!/bin/sh" > "$pkgdir/usr/bin/setup_dxvk$1"
    echo "winetricks --force /usr/share/dxvk/x$1/setup_dxvk_aur.verb" >> "$pkgdir/usr/bin/setup_dxvk$1"
    chmod +x "$pkgdir/usr/bin/setup_dxvk$1"
}

package_dxvk-wine64-git() {
    arch=('x86_64')
    conflicts=("dxvk-win64-bin" "dxvk-win64-git" "dxvk-bin")
    provides=("dxvk" "dxvk64")
    optdepends=('dxvk32: 32-bit support')
    _package_dxvk 64
}

package_dxvk-wine32-git() {
    arch=('i686' 'x86_64')
    conflicts=("dxvk-win32-bin" "dxvk-win32-git" "dxvk-bin")
    depends=('lib32-vulkan-icd-loader' 'wine>=3.14' 'winetricks')
    optdepends=('dxvk64: 64-bit support')
    provides=("dxvk" "dxvk32")
    _package_dxvk 32
}

package_dxvk-git() {
    pkgdesc="Dummy package to smooth the transition to the split packages"
    depends=("dxvk-wine32-git" "dxvk-wine64-git")
}
