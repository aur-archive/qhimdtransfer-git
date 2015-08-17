# Maintainer: Kevin Chabowski <kevin@kch42.de>

# Thanks to Fritz Reichwald for his contributions to this PKGBUILD (man pages
# and udev rules).

pkgname=qhimdtransfer-git
pkgver=20140120
pkgrel=1
pkgdesc="A QT-Based tool for reading and writing music to / from MiniDiscs"
arch=('i686' 'x86_64')
url="https://wiki.physik.fu-berlin.de/linux-minidisc/"
license=('GPL2' 'LGPL')
depends=('qt4' 'taglib' 'libgcrypt' 'glib2' 'sox' 'libmad' 'libusb' 'libid3tag')
makedepends=('git' 'qtcreator')
provides=('qhimdtransfer' 'himdcli' 'netmdcli')

_gitroot="git://z6.physik.fu-berlin.de/linux-minidisc.git"
_gitname="linux-minidisc"
build() {
	cd "$srcdir"
	msg "Getting sources via git..."
	
	if [ -d "$_gitname" ]; then
		cd $_gitname && git pull origin
	else
		git clone $_gitroot
		cd "$_gitname"
	fi
	
	msg "Sucessfully checked out sources via git."
	msg "Starting build..."
	
	qmake-qt4 -r
	make
}

package() {
	cd "$srcdir/$_gitname"
	make INSTALL_ROOT="${pkgdir}" install
	
	mkdir -p $pkgdir/etc/udev/rules.d	
	cp netmd/etc/netmd.rules $pkgdir/etc/udev/rules.d/70-netmd.rules	
	chmod 750 $pkgdir/etc/udev/rules.d/70-netmd.rules
	mkdir -p $pkgdir/usr/share/man/man1	
	gzip -c docs/netmdcli.1 > $pkgdir/usr/share/man/man1/netmdcli.1.gz	
	gzip -c docs/himdcli.1 > $pkgdir/usr/share/man/man1/himdcli.1.gz	
	gzip -c docs/qhimdtransfer.1 > $pkgdir/usr/share/man/man1/qhimdtransfer.1.gz
}
