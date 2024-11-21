# Maintainer: Caleb Maclennan <caleb@alerque.com>
# Contributor: ThatOneCalculator <kainoa@t1c.dev>
# Contributor: Brenno Lemos <brenno@syndel.is>
# Contributor: Gabriel Fox <inbox@gabrielfox.dev>

pkgname=hyprland
pkgver=0.45.2
pkgrel=1
pkgdesc='a highly customizable dynamic tiling Wayland compositor'
arch=(x86_64 aarch64)
url="https://github.com/hyprwm/${pkgname^}"
license=(BSD-3-Clause)
depends=(cairo # libcairo.so
         aquamarine libaquamarine.so
         gcc-libs # libgcc_s.so libstdc++.so
         glibc # libc.so libm.so
         glib2 libgobject-2.0.so
         glslang
         hyprcursor libhyprcursor.so
         hyprlang libhyprlang.so
         hyprutils libhyprutils.so
         libdisplay-info libdisplay-info.so
         libdrm # libdrm.so
         libglvnd libEGL.so libGLESv2.so libOpenGL.so
         libinput # libinput.so
         libliftoff libliftoff.so
         libx11
         libxcb # libxcb-dri3.so libxcb-present.so libxcb-render.so libxcb-res.so libxcb-shm.so libxcb.so libxcb-xfixes.so libxcb-xinput.so
         libxcomposite # libxcb-composite.so
         libxcursor
         libxfixes
         libxkbcommon libxkbcommon.so
         libxrender
         mesa # libgbm.so
         opengl-driver
         pango libpango-1.0.so libpangocairo-1.0.so
         pixman libpixman-1.so
         seatd libseat.so
         systemd-libs libsystemd.so
         tomlplusplus libtomlplusplus.so libudev.so
         vulkan-icd-loader
         wayland libwayland-client.so libwayland-server.so
         wayland-protocols
         xcb-proto
         xcb-util
         xcb-util-errors # libxcb-errors.so
         xcb-util-image
         xcb-util-keysyms
         xcb-util-renderutil # libxcb-render-util.so 
         xcb-util-wm # libxcb-ewmh.so  libxcb-icccm.so 
         xorg-xwayland)
makedepends=(cmake
             hyprwayland-scanner
             meson
             ninja
             vulkan-headers
             xorgproto)
optdepends=('cmake: to build and install plugins using hyprpm'
            'cpio: to build and install plugins using hyprpm'
            'meson: to build and install plugins using hyprpm')
provides=(wayland-compositor)
_archive="${pkgname^}-$pkgver"
source=("$_archive.tar.gz::$url/releases/download/v$pkgver/source-v$pkgver.tar.gz")
sha256sums=('d70231021f44980ef1c587e3adcb13471cface8ae580f0b503628b0391a716cb')

prepare() {
	ln -sf hyprland-source "$_archive"
	cd "$_archive"
	sed -i -e '/^release:/{n;s/-D/-DCMAKE_SKIP_RPATH=ON -D/}' Makefile

	# Workaround for https://gitlab.archlinux.org/archlinux/packaging/packages/hyprland/-/issues/15
	rm -fv scripts/generateVersion.sh
}

build() {
	cd "$_archive"
	make release PREFIX=/usr
}

package() {
	cd "$_archive"
	make DESTDIR="$pkgdir" install
	rm -fv "$pkgdir/usr/include/hyprland/src/version.h.in"
	# Maybe we should keep these?
	rm -frv "$pkgdir/usr/include/hyprland/protocols"
	# Drop this line when hyprland-portals.conf is removed from xdg-desktop-portal-hyprland
	rm -frv "$pkgdir/usr/share/xdg-desktop-portal"
	install -Dm0644 -t "$pkgdir/usr/share/licenses/$pkgname/" LICENSE
}
