# Maintainer: Bruno Silva <brunofernandes at ua dot pt>
# Contributor: Thor K. Høgås <thor that-circular-a roht dot no>

pkgname='omnetpp-qt'
pkgver=6.0pre7
pkgrel=1
_pkgname='omnetpp'
pkgdesc='OMNeT++ Discrete Event Simulator. OMNeT++ is an extensible, modular, component-based C++ simulation library and framework, primarily for building network simulators: QT version'
url='http://www.omnetpp.org'
license=('custom')
depends=(libxml2 qt5-base tcl jdk-openjdk openmpi python libpcap doxygen graphviz clang openscenegraph)
makedepends=(sh wget cmake bison flex perl)
optdepends=(
	'python-numpy: analysing simulation recordings' 
	'python-matplotlib: analysing simulation recordings'
	'python-pandas: analysing simulation recordings'
	'python-posix_ipc: analysing simulation recordings'
	'osgearth')
arch=('i686' 'x86_64')
provides=('omnetpp')
conflicts=('omnetpp')
install=omnetpp-qt.install

source=(OMNeT++.desktop
        omnetpp.sh
        "${_pkgname}-${pkgver}-src-linux.tgz::https://github.com/omnetpp/omnetpp/releases/download/omnetpp-${pkgver}/omnetpp-${pkgver}-src-linux.tgz")

sha512sums=('a5772a605592ed2db839609f8298d1d71fb9141eb1b30dac584b788414dfe49b250ba803351a3a84f90c6b89f8e09e7b129a037af17c9b94c22dff2003a5edd8'
            '2b98b46b290440299ef9fd8f4fd9a1841b843ebc73e4d57de6a805d28e77841628320e078087c5d137c522db4fd2cb528975e33b03cc69c60df8aefbf362e8e2'
            '338175dfc52998c4cf1b770224f510cc2937ea0dcde690eeb5c59d0cea5a0c52e6e3d36c0b60dbf5280a4071e4a160b4e2f8632f31e09acaf3a5e9f122cb5654')

build() {
	cd ${srcdir}/${_pkgname}-${pkgver}
	echo WITH_OSGEARTH=no >> configure.user
	# Fix configure script
	sed -i "2152 a ac_configure_args=$(echo $ac_configure_args | sed s/\'//g)" configure

	./configure --prefix=/opt --libdir=/opt/lib --libexecdir=/opt/lib
	PATH=${srcdir}/${_pkgname}-${pkgver}/bin:$PATH
	LD_LIBRARY_PATH=${srcdir}/${_pkgname}-${pkgver}/lib:$LD_LIBRARY_PATH

	make
}

package() {
	# Install build to /opt
	cd ${srcdir}
	mkdir -p "${pkgdir}"/opt
	mv  "${_pkgname}-${pkgver}" ${pkgdir}/opt/${_pkgname} || return 1

	# run OMNeT++ as a normal user
	touch ${pkgdir}/opt/${_pkgname}/ide/error.log
	chmod 777 ${pkgdir}/opt/${_pkgname}/ide/error.log

	# copy profile.d file
	mkdir -p ${pkgdir}/etc/profile.d/
	cp omnetpp.sh ${pkgdir}/etc/profile.d/
	
	# copy desktop shortcut
	mkdir -p ${pkgdir}/usr/share/applications/
	cp OMNeT++.desktop ${pkgdir}/usr/share/applications/

	# Install License
	cd ${pkgdir}/opt/${_pkgname}/doc
	install -D -m644 License "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}

