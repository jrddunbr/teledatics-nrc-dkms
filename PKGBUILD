pkgname=teledatics-nrc-dkms
pkgver=1.34.11
pkgrel=1
pkgdesc="Teledatics NRC DKMS driver for the TD-XPAH development board"
arch=('any')
url=""
license=('BSD', 'GPL2')
depends=('teledatics-spi-ft232h-dkms')
source=("${srcdir}/${pkgname}::git+https://github.com/teledatics/nrc7292_sw_pkg")
sha256sums=('SKIP')

prepare() {
    # REMAKE_INITRD has been removed from dkms on Arch Linux, it just throws a warning and does nothing
    sed -i '/REMAKE_INITRD=yes/d' "${srcdir}/usr/src/nrc-${pkgver}/dkms.conf"

    # Change expected module filename
    sed -i 's/BUILT_MODULE_NAME\[0\]="\$PACKAGE_NAME"/BUILT_MODULE_NAME\[0\]="nrc"/' "${srcdir}/usr/src/nrc-${pkgver}/dkms.conf"
}

package() {
  # Copy all dkms source code
  mkdir -p "${pkgdir}/usr/src/nrc-${pkgver}"
  cp "${srcdir}/usr/src/nrc-${pkgver}/" "${pkgdir}/usr/src/" -r

  # Install modprobe.d file
  install -Dm644 "${srcdir}/etc/modprobe.d/nrc.conf" "${pkgdir}/etc/modprobe.d/nrc.conf"

  # Install firmware
  install -Dm644 "${srcdir}/lib/firmware/nrc7292_bd.dat" "${pkgdir}/usr/lib/firmware/nrc7292_bd.dat"
  install -Dm644 "${srcdir}/lib/firmware/nrc7292_cspi.bin" "${pkgdir}/usr/lib/firmware/nrc7292_cspi.bin"

  # Install scripts
  install -Dm755 "${srcdir}/usr/local/bin/nrc_busno.sh" "${pkgdir}/usr/local/bin/nrc_busno.sh"
  install -Dm755 "${srcdir}/usr/local/bin/nrc_gpiono.sh" "${pkgdir}/usr/local/bin/nrc_gpiono.sh"
  install -Dm755 "${srcdir}/usr/local/bin/nrc_load_module.sh" "${pkgdir}/usr/local/bin/nrc_load_module.sh"

  # Install license
  install -Dm644 "${srcdir}/usr/share/doc/nrc-dkms/copyright" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}