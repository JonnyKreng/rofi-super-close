pkgname=rofi-super-close-git
pkgver=1.7.3.r77.gc4e8a6c3
pkgrel=1
pkgdesc='A window switcher, run dialog and dmenu replacement'
arch=('x86_64')
url='https://github.com/JonnyKreng/rofi-super-close'
license=('MIT')
depends=(
  'cairo' 'flex' 'freetype2' 'libjpeg' 'librsvg' 'libx11' 'libxcb'
  'libxdg-basedir' 'libxft' 'libxkbcommon' 'libxkbcommon-x11' 'pango'
  'startup-notification' 'xcb-util' 'xcb-util-wm' 'xcb-util-xrm'
)
makedepends=('git' 'meson' 'ninja')
checkdepends=('check')
provides=("${pkgname/-git}" "rofi")
conflicts=("${pkgname/-git}" "rofi")
source=(
  'git+https://github.com/JonnyKreng/rofi-super-close#branch=next'
  'git+https://github.com/sardemff7/libgwater'
  'git+https://github.com/sardemff7/libnkutils'
)
sha256sums=('SKIP' 'SKIP' 'SKIP')

pkgver() {
  cd "${pkgname/-git}"

  git describe --long --tags \
    | sed 's/-/.r/;s/-/./'
}

prepare() {
  cd "${pkgname/-git}"
  for module in libgwater libnkutils; do
    local submodule="subprojects/${module}"
    git submodule init "${submodule}"
    git config "submodule.${submodule}.url" "${srcdir}/${module}"
    git submodule update "${submodule}"
  done
  cd "${srcdir}"

  meson setup "${pkgname/-git}" build \
    --buildtype release               \
    --prefix /usr
}

build() {
  ninja -C build
}

check() {
  ninja -C build test
}

package() {
  DESTDIR="${pkgdir}" ninja -C build install

  cd "${pkgname/-git}"
  install -Dm644 COPYING "${pkgdir}/usr/share/licenses/rofi/COPYING"
  install -Dm755 Examples/*.sh -t "${pkgdir}/usr/share/doc/rofi/examples"
}
