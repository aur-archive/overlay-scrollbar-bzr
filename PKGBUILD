# Maintainer: quequotion <quequotion@bugmenot.com>
# Contributor: Xiao-Long Chen <chenxiaolong@cxl.epac.to>

pkgname=({overlay-scrollbar,liboverlay-scrollbar{,3}}-bzr)
pkgbase=overlay-scrollbar-bzr
pkgver=r391
pkgrel=2
pkgdesc="Ayatana Scrollbars, pointer-triggered overlay scrollbars to save screen real-estate."
arch=('i686' 'x86_64')
url="https://launchpad.net/ayatana-scrollbar"
license=('LGPL')
groups=('unity')
makedepends=('gtk2-ubuntu' 'gtk3')
options=('!emptydirs')
extrafiles=('overlay-scrollbar.install')
source=('bzr+lp:overlay-scrollbar')
sha512sums=('SKIP')

pkgver() {
  cd overlay-scrollbar

  printf "r%s" "$(bzr revno)"
}

build() {
  cd "${srcdir}/overlay-scrollbar"

  autoreconf -vfi

  [[ -d build-gtk2 ]] || mkdir build-gtk2
  pushd build-gtk2
  ../configure --prefix=/usr --sysconfdir=/etc --with-gtk=2 --disable-static
  make
  popd

  [[ -d build-gtk3 ]] || mkdir build-gtk3
  pushd build-gtk3
  ../configure --prefix=/usr --sysconfdir=/etc --with-gtk=3 --disable-static
  make
  popd
}

package_overlay-scrollbar-bzr() {
  pkgdesc+="Configuration and metapackage"
  depends=('dconf' liboverlay-scrollbar{,3})
  provides=(overlay-scrollbar{,-bzr}="${pkgver}")
  conflicts=(overlay-scrollbar)
  install=overlay-scrollbar.install

  cd "${srcdir}/overlay-scrollbar/build-gtk3/"
  make DESTDIR="${pkgdir}/" install

  rm -rvf "${pkgdir}/usr/lib/"

  # Put X11 startup script in correct directory
  install -dm755 "${pkgdir}/etc/X11/xinit/xinitrc.d/"
  mv "${pkgdir}/etc/X11/Xsession.d/81overlay-scrollbar" \
    "${pkgdir}/etc/X11/xinit/xinitrc.d/"
  chmod 755 "${pkgdir}/etc/X11/xinit/xinitrc.d/81overlay-scrollbar"
}

package_liboverlay-scrollbar3-bzr() {
  pkgdesc+=" gtk3"
  depends=('cairo' 'gtk3')
  provides=(liboverlay-scrollbar{3,-gtk3}{,-bzr}="${pkgver}")
  conflicts=('liboverlay-scrollbar3')

  cd "${srcdir}/overlay-scrollbar/build-gtk3/"
  make DESTDIR="${pkgdir}/" install

  rm -rvf "${pkgdir}/etc/"
  rm -rvf "${pkgdir}/usr/share/glib-2.0/"
}

package_liboverlay-scrollbar-bzr() {
  pkgdesc+=" gtk2"
  depends=('gtk2-ubuntu')
  provides=(liboverlay-scrollbar{,-gtk2}{,-bzr}="${pkgver}")
  conflicts=('liboverlay-scrollbar')

  cd "${srcdir}/overlay-scrollbar/build-gtk2/"
  make DESTDIR="${pkgdir}/" install

  rm -rvf "${pkgdir}/etc/"
  rm -rvf "${pkgdir}/usr/share/glib-2.0/"
}

