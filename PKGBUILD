pkgbase=dxvk-git
pkgname=('dxvk-win64-git' 'dxvk-win32-git' 'dxvk-git')
pkgver=0.81-async
pkgrel=1
pkgdesc="A Vulkan-based compatibility layer for Direct3D 10/11 which allows running 3D applications on Linux using Wine."
arch=('x86_64' 'i686')
url="https://github.com/doitsujin/dxvk"
license=('zlib/libpng')
makedepends=('ninja' 'meson>=0.43' 'glslang' 'mingw-w64-gcc' 'git' 'wine')
options=(!strip !buildflags staticlibs)

source $PWD/customization.cfg

source=($pkgbase::"git+https://github.com/doitsujin/dxvk.git"
    "setup_dxvk_aur.verb"
    "dxvk-async.patch"
)

md5sums=('SKIP'
         '63d0a0ac0927d01d256bf7d781b5111b'
         '837eb795c77dc52a13586a30fe0729e2')

pkgver() {
        cd "$pkgbase"
        git describe | sed s/"-"/"_"/g | sed 's/^v\(.*\)/\1/'
}

prepare() {
	cd 'dxvk-git'

	# Checkout different commit
	if [ $_use_dxvk == "true" ]; then
		git reset --hard "${_dxvk_commit}"
	fi

	# Apply patch
	patch -Np1 < ../'dxvk-async.patch'
	
	cd ..
}

build() {
	"$pkgbase"/package-release.sh $pkgver $PWD --no-package
}

_package_dxvk() {
        destdir="/usr/share/dxvk/"
	mkdir -p "$pkgdir/$destdir"
	cp -rv dxvk-$pkgver/x$1 "$pkgdir/$destdir"
	extension=".dll"
	for libname in "d3d11" "dxgi" "d3d10" "d3d10_1" "d3d10core"; do
            if [ ! -f "$pkgdir"/$destdir/x$1/$libname$extension ] ; then
                    echo "Missing file: $libname$extension, build was unsuccessful"
                    return 1
            fi
        done
        mkdir -p "$pkgdir/usr/bin"
        cat setup_dxvk_aur.verb | sed s/"DXVK_ARCH=64"/"DXVK_ARCH=$1"/g > "$pkgdir/$destdir/x$1/setup_dxvk_aur.verb"
	echo "winetricks --force $destdir/x$1/setup_dxvk_aur.verb" > "$pkgdir/usr/bin/setup_dxvk$1"
	chmod +x "$pkgdir/usr/bin/setup_dxvk$1"
}

package_dxvk-win64-git() {
        arch=('x86_64')
        conflicts=("dxvk-win64-bin")
        provides=("dxvk" "dxvk64")
        depends=('vulkan-icd-loader' 'wine>=3.10' 'winetricks')
        conflicts=("dxvk-git<$pkgver")
        replaces=("dxvk-git")
        _package_dxvk 64
}
package_dxvk-win32-git() {
        arch=('i686' 'x86_64')
        conflicts=("dxvk-win32-bin")
        provides=("dxvk" "dxvk32")
        depends=('lib32-vulkan-icd-loader' 'wine>=3.10' 'winetricks')
        conflicts=("dxvk-git<$pkgver")
        replaces=("dxvk-git")
        _package_dxvk 32
}
package_dxvk-git() {
	pkgdesc="Dummy package to smooth the transition to the split packages"
	depends=("dxvk-win32-git" "dxvk-win64-git")
}
