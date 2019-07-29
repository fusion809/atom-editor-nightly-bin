# Maintainer: George Rawlinson <george@rawlinson.net.nz>
# Contributor: Andre Miranda <andreldm1989 AT gmail DOT com>
# Contributor: Tom Bu <tom.bu AT members.fsf.org>
# Contributor: John Reese <john@noswap.com>
# Contributor: Jordan J Klassen <forivall@gmail.com>
# Contributor: Danny Arnold <despair.blue at gmail dot com>

pkgname=atom-editor-nightly-bin
pkgver=20190729
pkgrel=1
pkgdesc="Hackable text editor built on Electron (official precompiled binary)"
arch=('x86_64')
url="https://github.com/atom/atom"
license=('MIT')
provides=()
options=(!strip !emptydirs)
depends=('git' 'gconf' 'gtk2' 'libnotify' 'libxtst' 'nss' 'python2' 'xdg-utils' 'desktop-file-utils' 'alsa-lib' 'libgnome-keyring' 'libxss')
optdepends=('gvfs')
conflicts=()
install=$pkgname.install
source=("atom-amd64-nighty-${pkgver}.deb::https://atom.io/download/deb?channel=nightly"
         "LICENSE::https://raw.githubusercontent.com/atom/atom/master/LICENSE.md"
         atom-editor-nightly-bin.install
         atom-python.patch
         startupwmclass.patch
)
sha512sums=('5a83c2c2ef8b9d7c8773ba61f82b514da76ad2e69989455cfd4ad76e4a939e021d16db6106dd9408e810d7239e6806e85ec293eb890818bb527f099ef107fe2a'
            '4946629b14a57a57a3849fc3ff7f43779b55e1883383949212ccb6cb7bb4867edffb927f9d85cb2717b932c4f5162140d421e19df031edc7ea384ee93d93b2a4'
            'e30f7e4812898b80c079ba419e0cb37522c2e154ef7fdd6dda3da06dcbcaadc42016dd3d3b8caf206b842a2b9e3b954e537626d72337c56f05365a733627ce6c'
            '4d745796fa407944fcf7412c997b7c300cb53aa13818ee1241e92676b1f7215a67d1b3efd423699f1b13a520480c99bf9fa5ac615f007eff6b9a7b3d75a1246d'
            'da92c4c77240dba1ed27cd5a2d02ab76d7789c07179a53cd643d14240df4a6d9903a371510c3ecf4ed9d2601112bc14de00d74030770de342c51eb4a0f627bd0')

pkgver() {
   echo $(date +%Y%m%d)
}

prepare() {
  cd $srcdir
  # Extract data
  bsdtar xf data.tar.xz

  # Apply patches
  patch -sp1 < "${srcdir}"/atom-python.patch
  patch -sp1 < "${srcdir}"/startupwmclass.patch

  # Modify package to use python2 instead of python3
  sed -i -e 's|env PYTHON=python2 GTK_IM_MODULE= QT_IM_MODULE= XMODIFIERS= /usr/share/atom/atom|/usr/bin/atom|' \
	 -e "s|ATOM_DISABLE_SHELLING_OUT_FOR_ENVIRONMENT=false|PYTHON=python2 ATOM_DISABLE_SHELLING_OUT_FOR_ENVIRONMENT=false|g" usr/share/applications/atom-nightly.desktop
  sed -i 's|python|python2|' usr/share/atom-nightly/resources/app/apm/bin/python-interceptor.sh
}

package() {
  # Recursively remove group's write permission before moving to package directory
  chmod -R g-w usr
  mv usr "${pkgdir}"

  # Add LICENSE
  install -Dm644 -t "${pkgdir}/usr/share/licenses/${pkgname}" LICENSE
}
