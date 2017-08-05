# Maintainer: Eric Vidal <eric@obarun.org>
# based on the original https://aur.archlinux.org/cgit/aur.git/log/PKGBUILD?h=eudev-git
# 						Maintainer: Alexey D. <lq07829icatm@rambler.ru>
# 						Contributor: Ivailo Monev <xakepa10@gmail.com>

pkgbase=eudev
pkgname=eudev-obarun
pkgver=3.2.2
pkgrel=1
pkgdesc="The userspace dev tools (udev) forked by Gentoo"
arch=(x86_64)
url="https://github.com/gentoo/eudev"
license=('GPL')
depends=('kbd' 'kmod' 'hwids' 'util-linux')
makedepends=('gobject-introspection' 'gperf' 'gtk-doc' 'intltool' 'kmod')
optdepends=('upower-pm-utils: pm-utils support'
			'libgudev: GObject bindings for libudev')
conflicts=('eudev' 'eudev-git' 'udev' 'eudev-libgudev')
replaces=('eudev' 'udev')
provides=('eudev')
backup=('etc/udev/udev.conf' 'usr/lib/udev/rules.d/80-net-name-slot.rules')
options=(!makeflags !libtool)
source=("http://dev.gentoo.org/~blueness/eudev/eudev-${pkgver}.tar.gz"
		'initcpio_hooks'
		'initcpio_install'
		'udev-hwdb.hook')
sha256sums=('3e4c56ec2fc1854afd0a31f3affa48f922c62d40ee12a0c1a4b4f152ef5b0f63'
            '892ce43218e0a458981bbce451252c8987dc398e60b8de288e7542b8f2409c13'
            '77dd1fd318b4456409aceb077f060b87944defb07cf39d29ad1968dc6f361875'
            '846e9ddbb95c8394ba7efe75107cc1308426921bc042f5d6b48fa4c2dcbac151')
validpgpkeys=('6DD4217456569BA711566AC7F06E8FDE7B45DAAC') # Eric Vidal

prepare(){
	cd "${srcdir}/${pkgbase}-${pkgver}"
	sed -e 's/GROUP="dialout"/GROUP="uucp"/' \
		-e 's/GROUP="tape"/GROUP="storage"/' \
		-e 's/GROUP="cdrom"/GROUP="optical"/' \
		-i rules/*.rules
}

build() {
	cd "${srcdir}/${pkgbase}-${pkgver}"

  	if [ -f "Makefile" ];then
   	 msg2 "Cleaning up..."
   	 make clean
 	fi

	#./autogen.sh
	./configure \
		--prefix=/usr \
		--with-rootprefix=/usr \
		--sysconfdir=/etc \
		--libdir=/usr/lib \
		--sbindir=/usr/bin \
		--enable-introspection \
		--enable-kmod \
		--enable-manpages \
		--enable-split-usr 
	make
}

package() {
	cd "${srcdir}/${pkgbase}-${pkgver}"
	make DESTDIR="${pkgdir}" install

	install -Dm644 "${srcdir}/initcpio_hooks" "${pkgdir}/usr/lib/initcpio/hooks/udev"
	install -Dm644 "${srcdir}/initcpio_install" "${pkgdir}/usr/lib/initcpio/install/udev"
	install -Dm644 "${srcdir}/udev-hwdb.hook" "${pkgdir}/usr/share/libalpm/hooks/udev-hwdb.hook"
	
	# input group is not used in Arch Linux at this moment
	#sed '/GROUP="input"/d' -i "${pkgdir}/usr/lib/udev/rules.d/50-udev-default.rules"

  	# Getting udev version
  	udevver=$(grep UDEV_VERSION configure.ac | egrep -o "[0-9]{3}")
  	provides+=("udev=$udevver")
}
