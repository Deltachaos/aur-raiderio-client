# Maintainer: Deltachaos <mr@deltachaos.de>

pkgname=raiderio-client-appimage
_pkgapp=raiderio-client
pkgver=4.1.1
pkgrel=1
pkgdesc="raider.io desktop client for Linux"
arch=('x86_64')
depends=("fuse2")
url="https://raider.io/"
source=("$_pkgapp.AppImage::https://raider.io/client/download/linux"
        'start')
license=('custom' 'MIT' 'custom:chromium-licenses')
options=(!strip)
# Skip checksum check for the RaiderIO binary, to avoid breakage on updates
sha512sums=('SKIP'
            'd81f917aba4da97441462abf6e9dcb0240a47038f69d5efd715a5f41f81e5930b155febaabd6b551f9183f37a00b75e47ff58e3cef8108fda2669898cf1abdd6')

pkgver() {
    cd ${srcdir}
    chmod +x ${srcdir}/${_pkgapp}.AppImage
    ${srcdir}/${_pkgapp}.AppImage --appimage-extract >/dev/null
    cat ${srcdir}/squashfs-root/${_pkgapp}.desktop | grep 'X-AppImage-Version' | sed 's!^X-AppImage-Version=!!g'
}

package() {
    cd ${srcdir}
    chmod +x ${srcdir}/${_pkgapp}.AppImage
    ./${_pkgapp}.AppImage --appimage-extract >/dev/null
    sed -i 's/Exec=.*/Exec=\/usr\/bin\/'${_pkgapp}' %U/' squashfs-root/${_pkgapp}.desktop

    install -Dm755 ${_pkgapp}.AppImage "${pkgdir}/opt/${_pkgapp}/RaiderIO.AppImage"
    install -Dm755 "start" "${pkgdir}/usr/bin/${_pkgapp}"
    install -dm755 "${pkgdir}/usr/share/applications/"
    install -dm755 "${pkgdir}/usr/share/icons/hicolor/scalable/apps/"
    install -dm755 "${pkgdir}/usr/share/licenses/${_pkgapp}/"

    cp -r --no-preserve=mode,ownership "${srcdir}/squashfs-root/usr/share/icons/hicolor/scalable/${_pkgapp}.svg" "${pkgdir}/usr/share/icons/hicolor/scalable/apps/"
    cp --no-preserve=mode,ownership "${srcdir}/squashfs-root/${_pkgapp}.desktop" "${pkgdir}/usr/share/applications/"
    for i in ${srcdir}/squashfs-root/LICENSE.*; do 
      cp --no-preserve=mode,ownership "${i}" "${pkgdir}/usr/share/licenses/${_pkgapp}"
    done
}
