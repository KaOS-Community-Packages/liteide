#
pkgname=liteide
pkgver=25
pkgrel=1
pkgdesc='Simple, open source, cross-platform Go IDE in Qt5'
license=('LGPL')
arch=('x86_64')
url='https://github.com/visualfc/liteide'
depends=('go' 'qt5-webkit')
makedepends=('qt5-base' 'go' 'gendesk' 'git' 'mercurial' )
options=('!strip' '!emptydirs')
source=("https://github.com/visualfc/${pkgname}/archive/x${pkgver}.tar.gz")
md5sums=('7f64587eee411fd7809b46c49ae5b270')

prepare() {
	cd "${srcdir}/${pkgname}-x${pkgver}"
	chmod +x build/*.sh
	sed -i 's/qmake/qmake-qt5/g' build/build_linux.sh
	sed -i 's/^GOROOT/#GOROOT/g' liteidex/os_deploy/linux/liteenv/linux64.env
}

build() {
  cd "${srcdir}/${pkgname}-x${pkgver}/build"
  export QTDIR=/usr
  export GOPATH=$(pwd)/go
  mkdir -p go
  ./update_pkg.sh
  ./build_linux.sh
  gendesk -f -n --name 'LiteIDE' --pkgname "${pkgname}" --pkgdesc "${pkgdesc}"
}

package() {
	cd "${srcdir}/${pkgname}-x${pkgver}"

	mkdir -p "${pkgdir}/usr/share/${pkgname}" "${pkgdir}/usr/lib/${pkgname}"

	cp -r "build/${pkgname}/bin" "${pkgdir}/usr"
	cp -r liteidex/deploy/* liteidex/os_deploy/* "${pkgdir}/usr/share/${pkgname}"
	cp -r liteidex/liteide/lib/liteide/* "${pkgdir}/usr/lib/${pkgname}"


	install -Dm644 "build/${pkgname}.desktop" "${pkgdir}/usr/share/applications/${pkgname}.desktop"
	install -Dm644 "liteidex/src/liteapp/images/${pkgname}128.png" "${pkgdir}/usr/share/pixmaps/${pkgname}.png"

	mv "${pkgdir}/usr/share/liteide/linux/liteenv" "$pkgdir/usr/share/liteide/liteenv"
}