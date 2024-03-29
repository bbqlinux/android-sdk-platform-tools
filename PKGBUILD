# Maintainer: Daniel Hillenbrand <codeworkx [at] bbqlinux [dot] org>
# Contributor: Gordin <9ordin @t gmail dot com>

pkgname=android-sdk-platform-tools
pkgver=r18.0.1
pkgrel=1
pkgdesc='Platform-Tools for Google Android SDK (adb and fastboot)'
arch=('i686' 'x86_64')
url="http://developer.android.com/sdk/index.html"
license=('custom')
depends=('gcc-libs' 'zlib' 'ncurses')
if [[ $CARCH = x86_64 ]]; then
  depends=('lib32-gcc-libs' 'lib32-zlib' 'lib32-ncurses')
fi
provides=('adb')
conflicts=('adb')
_sdk=android-sdk
_tools="opt/${_sdk}/tools"
_ptools="opt/${_sdk}/platform-tools"

source=("http://dl-ssl.google.com/android/repository/platform-tools_${pkgver}-linux.zip"
        "adb.service"
        "license.html")
sha256sums=('ae09ea0e248f07502d3e68ce7dc6698f97abffa30cdf499af252cb390aa788ac'
            '1c219abea7584ae13f3f76b04e269ef21c1699d6bd29b7615523f927a9d10deb'
            'a7f3a259290ae6a5dc61bd34ecae36e2b7e2f644865ddc3c7fde5d248b8a7cef')

package() {
  install -Dm644 "${srcdir}/adb.service" "${pkgdir}/usr/lib/systemd/system/adb.service"
  install -Dm644 "${srcdir}/license.html" usr/share/licenses/$pkgname/license.html
  cd "$pkgdir"
  mkdir -p opt etc/profile.d
  echo 'export PATH=$PATH:/opt/android-sdk/platform-tools' > etc/profile.d/${pkgname}.sh
  echo 'setenv PATH ${PATH}:/opt/android-sdk/platform-tools' > etc/profile.d/${pkgname}.csh
  chmod 755 etc/profile.d/${pkgname}.{csh,sh}
  mkdir -p opt/$_sdk
  cp -a "$srcdir/platform-tools" "$pkgdir/opt/$_sdk/platform-tools"

  # Fix permissions
  chmod -R ugo+rX "${pkgdir}/opt"
  chmod g+rwX "${pkgdir}/opt/${_sdk}/platform-tools"
  chown -R android "${pkgdir}/opt/${_sdk}/platform-tools"
  chgrp -R users "${pkgdir}/opt/${_sdk}/platform-tools"
}
