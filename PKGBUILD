pkgname=liteide
pkgver=38.3
pkgrel=1
pkgdesc='Simple, open source, cross-platform Go IDE in Qt5'
license=('LGPL')
arch=('x86_64')
url='https://github.com/visualfc/liteide'
depends=('go' 'qt5-base')
makedepends=('go' 'git' 'mercurial')
options=('!strip' '!emptydirs')
source=("https://github.com/visualfc/${pkgname}/archive/x${pkgver}.tar.gz")
sha256sums=('d1a6f11b628cdd2ed8b808acb84cb6c098144c24d968ecfff0062731c3e73825')

prepare() {
	cd "${srcdir}/${pkgname}-x${pkgver}"

	chmod +x build/*.sh
	sed -i 's/qmake/qmake-qt5/g' build/build_linux.sh
	sed -i 's/^GOROOT/#GOROOT/g' liteidex/os_deploy/linux/liteenv/linux64.env

	# Fix the libpng warning: iCCP: known incorrect sRGB profile
	find . -type f -iname "*.png" -exec mogrify -strip '{}' \;

	sed -i 's/"CONFIG+=release"/"CONFIG+=release" "QMAKE_LFLAGS+=-Wl,-z,relro,-z,now" "QMAKE_CXXFLAGS+=-Wl,-z,relro,-z,now"/g' \
		build/build_linux.sh
}

build() {
	cd "${srcdir}/${pkgname}-x${pkgver}/build"

	mkdir -p go
	export QTDIR=/usr
	export GOPATH="$(pwd)/go"
#	export GOROOT=/usr/lib/go
	./update_pkg.sh
	./build_linux.sh
}

package() {
	cd "${srcdir}/${pkgname}-x${pkgver}"

	mkdir -p "${pkgdir}/usr/share/${pkgname}" "${pkgdir}/usr/lib/${pkgname}"

	cp -r "build/${pkgname}/bin" "${pkgdir}/usr"
	cp -r "liteidex/deploy/"* "liteidex/os_deploy/"* "${pkgdir}/usr/share/${pkgname}"
	cp -r "liteidex/liteide/lib/liteide/"* "${pkgdir}/usr/lib/${pkgname}"
	chmod -x "${pkgdir}/usr/lib/${pkgname}/plugins/"*

	install -Dm644 "liteidex/LICENSE.LGPL" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
	install -Dm644 "liteidex/LGPL_EXCEPTION.TXT" "${pkgdir}/usr/share/licenses/${pkgname}/LGPL_EXCEPTION"

	install -Dm644 "build/${pkgname}.desktop" "${pkgdir}/usr/share/applications/${pkgname}.desktop"
	install -Dm644 "liteidex/src/liteapp/images/${pkgname}128.png" "${pkgdir}/usr/share/icons/hicolor/128x128/apps/${pkgname}.png"
	install -d "${pkgdir}/usr/share/pixmaps"
	ln -s "/usr/share/liteide/welcome/images/liteide400.png" "${pkgdir}/usr/share/pixmaps/${pkgname}.png"

	mv "${pkgdir}/usr/share/liteide/linux/liteenv" "$pkgdir/usr/share/liteide/liteenv"
}
