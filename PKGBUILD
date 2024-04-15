# Maintainer: Caleb Maclennan <caleb@alerque.com>
# Contributor: ThatOneCalculator <kainoa@t1c.dev>
# Contributor: Brenno Lemos <brenno@syndel.is>
# Contributor: Gabriel Fox <inbox@gabrielfox.dev>

pkgname=hyprland
pkgver=0.39.0
pkgrel=1
pkgdesc='a highly customizable dynamic tiling Wayland compositor'
arch=(x86_64 aarch64)
url="https://github.com/hyprwm/${pkgname^}"
license=(BSD-3-Clause)
depends=(cairo # libcairo.so
         gcc-libs # libgcc_s.so libstdc++.so
         glibc # libc.so libm.so
         glib2 libgobject-2.0.so
         glslang
         hyprcursor libhyprcursor.so
         hyprlang libhyprlang.so
         libdisplay-info libdisplay-info.so
         libdrm # libdrm.so
         libglvnd libEGL.so libGLESv2.so libOpenGL.so
         libinput # libinput.so
         libliftoff libliftoff.so
         libx11
         libxcb # libxcb-dri3.so libxcb-present.so libxcb-render.so libxcb-res.so libxcb-shm.so libxcb.so libxcb-xfixes.so libxcb-xinput.so
         libxcomposite # libxcb-composite.so
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
         vulkan-validation-layers
         wayland libwayland-client.so libwayland-server.so
         wayland-protocols
         xcb-proto
         xcb-util
         xcb-util-errors # libxcb-errors.so
         xcb-util-image
         xcb-util-keysyms
         xcb-util-renderutil # libxcb-render-util.so 
         xcb-util-wm # libxcb-ewmh.so  libxcb-icccm.so 
         xorg-xinput
         xorg-xwayland)
makedepends=(cmake
             gdb
             meson
             ninja
             vulkan-headers
             xorgproto)
optdepends=('cmake: to build and install plugins using hyprpm'
            'cpio: to build and install plugins using hyprpm'
            'meson: to build and install plugins using hyprpm')
_archive="${pkgname^}-$pkgver"
source=("$_archive.tar.gz::$url/releases/download/v$pkgver/source-v$pkgver.tar.gz")
sha256sums=('2f4fc15bf18dc8148aee8cf110d3720adb07f5fe33c5eaafad11e4b21d8e068c')

prepare() {
	ln -sf hyprland-source "$_archive"
	cd "$_archive"
	sed -i -e '/^release:/{n;s/-D/-DCMAKE_SKIP_RPATH=ON -D/}' Makefile
}

build() {
	cd "$_archive"
	make release
}

package() {
	cd "$_archive"
	find src \( -name '*.h' -o -name '*.hpp' \) -exec install -Dm0644 {} "$pkgdir/usr/include/hyprland/{}" \;
	pushd subprojects/wlroots-hyprland/include
	find . -name '*.h' -exec install -Dm0644 {} "$pkgdir/usr/include/hyprland/wlroots/{}" \;
	popd
	pushd subprojects/wlroots-hyprland/build/include
	find . -name '*.h' -exec install -Dm0644 {} "$pkgdir/usr/include/hyprland/wlroots/{}" \;
	popd
	mkdir -p "$pkgdir/usr/include/hyprland/protocols"
	cp protocols/*-protocol.h "$pkgdir/usr/include/hyprland/protocols"
	pushd build
	cmake -DCMAKE_INSTALL_PREFIX=/usr ..
	popd
	install -Dm0644 -t "$pkgdir/usr/share/pkgconfig" build/hyprland.pc
	install -Dm0644 -t "$pkgdir/usr/share/man/man1" docs/{Hyprland,hyprctl}.1
	install -Dm0755 -t "$pkgdir/usr/bin/" build/Hyprland build/hyprctl/hyprctl build/hyprpm/hyprpm
	install -Dm0644 -t "$pkgdir/usr/share/$pkgname/" assets/*.png
	install -Dm0644 -t "$pkgdir/usr/share/wayland-sessions/" "example/$pkgname.desktop"
	install -Dm0644 -t "$pkgdir/usr/share/$pkgname/" "example/$pkgname.conf"
	install -Dm0644 -t "$pkgdir/usr/share/licenses/$pkgname/" LICENSE
        find subprojects/wlroots-hyprland/build -name 'libwlroots.so.*' -type f -execdir \
                install -Dm0755 -t "$pkgdir/usr/lib/" {} \;
}
