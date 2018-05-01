# Maintainer: Darcy Hu <hot123tea123@gmail.com>

## This PKGBUILD creates an Arch Linux package for the proprietary MATLAB application. A license from The MathWorks is needed in order to both build the package and to run MATLAB once the package is installed.
## In order to build the package the user must supply a plain text file installation key as file `matlab.fik`, the license file as `matlab.lic`, the software tarball as `matlab.tar`.
## To perform a network install set $_networkinstall to true.

pkgname=matlab
pkgver=9.2.0.538062
pkgrel=1
_instdir="/opt/${pkgname}"
pkgdesc='A high-level language for numerical computation and visualization'
arch=('x86_64')
url='http://www.mathworks.com'
license=(custom)
source=("file://matlab.tar"
		"file://matlab.fik"
		"file://matlab.lic")
md5sums=('SKIP'
		'SKIP'
		'SKIP')
PKGEXT='.pkg.tar'

# for networkinstall, change _networkinstall to true
_networkinstall=false

prepare() {
	msg2 'Extracting file installation key'
	_fik=$(grep -o [0-9-]* ${pkgname}.fik)

	msg2 'Modifying the installer settings'
	sed -i "s,^# destinationFolder=,destinationFolder=${pkgdir}/${_instdir}," "${srcdir}/${pkgname}/installer_input.txt"
	sed -i "s,^# agreeToLicense=,agreeToLicense=yes," "${srcdir}/${pkgname}/installer_input.txt"
	sed -i "s,^# mode=,mode=silent," "${srcdir}/${pkgname}/installer_input.txt"
	sed -i "s,^# fileInstallationKey=,fileInstallationKey=${_fik}," "${srcdir}/${pkgname}/installer_input.txt"

	if ${_networkinstall}; then
		sed -i "s,^# licensePath=,licensePath=${srcdir}/matlab.lic," "${srcdir}/${pkgname}/installer_input.txt"
	else
		sed -i "s,^# activationPropertiesFile=,activationPropertiesFile=${srcdir}/${pkgname}/activate.ini," "${srcdir}/${pkgname}/installer_input.txt"
		sed -i "s,^activateCommand=,activateCommand=activateOffline," "${srcdir}/${pkgname}/activate.ini"
		sed -i "s,^licenseFile=,licenseFile=${srcdir}/matlab.lic," "${srcdir}/${pkgname}/activate.ini"
	fi
}

package() {
	msg2 'Starting MATLAB installer'
	"${srcdir}/${pkgname}/install" -inputFile "${srcdir}/${pkgname}/installer_input.txt"

	msg2 'Installing license'
	install -D -m644 "${pkgdir}/${_instdir}/license_agreement.txt" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
