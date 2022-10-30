pkgname=teledatics-nrc-dkms
pkgver=1.34.11
pkgrel=1
pkgdesc="Teledatics NRC DKMS driver for the TD-XPAH development board"
arch=('any')
url=""
# BSD from debian copyright file, GPLv2 from source code, MIT from top level license.
license=('BSD', 'GPL2', 'MIT')
depends=('teledatics-spi-ft232h-dkms')
source=("${srcdir}/${pkgname}::git+https://github.com/teledatics/nrc-dkms#commit=fdbca06d91aa1ddde594ea5271a8d665c445b5c3" "dkms.conf" "nrc_gpiono.sh")
sha256sums=('SKIP' '381a2927423b5a98f82e190bcd36f6af2cb7071f6ed80c719362596228be599e' '5e0d446bb4768848c20f2d6dcda3c5abcb97a7c8c900ad02800e2f8f678ee922')

prepare() {
    sed -i 's/nrc/teledatics-nrc/' "${srcdir}/${pkgname}/misc/nrc.conf"
}

package() {
  # Copy all dkms source code
  mkdir -p "${pkgdir}/usr/src/${pkgname}-${pkgver}"
  cp "${srcdir}/${pkgname}/src/" "${pkgdir}/usr/src/${pkgname}-${pkgver}" -r

  # Install dkms config
  install -Dm644 "${srcdir}/dkms.conf" "${pkgdir}/usr/src/${pkgname}-${pkgver}/dkms.conf"

  # Install modprobe.d file
  install -Dm644 "${srcdir}/${pkgname}/misc/nrc.conf" "${pkgdir}/etc/modprobe.d/nrc.conf"

  # Install firmware
  install -Dm644 "${srcdir}/${pkgname}/misc/nrc7292_bd.dat" "${pkgdir}/usr/lib/firmware/nrc7292_bd.dat"
  install -Dm644 "${srcdir}/${pkgname}/misc/nrc7292_cspi.bin" "${pkgdir}/usr/lib/firmware/nrc7292_cspi.bin"

  # Install scripts
  install -Dm755 "${srcdir}/${pkgname}/misc/nrc_busno.sh" "${pkgdir}/usr/local/bin/nrc_busno.sh"
  # The following don't work on Arch Linux due to default kernel config incompatibilities
  # install -Dm755 "${srcdir}/${pkgdir}/misc/nrc_gpiono.sh" "${pkgdir}/usr/local/bin/nrc_gpiono.sh"
  # Load this other custom script I wrote up instead. TODO: Upstream this when working
  install -Dm755 "nrc_gpiono.sh" "${pkgdir}/usr/local/bin/nrc_gpiono.sh"
  install -Dm755 "${srcdir}/${pkgname}/misc/nrc_load_module.sh" "${pkgdir}/usr/local/bin/nrc_load_module.sh"

  # Install license
  install -Dm644 "${srcdir}/${pkgname}/debian/copyright" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}