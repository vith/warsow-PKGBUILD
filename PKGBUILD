pkgname=('warsow-client' 'warsow-server')
pkgbase='warsow'
pkgver=2.1
_pkgver=${pkgver/./}
pkgrel=1
pkgdesc='Free online multiplayer competitive FPS based on the Qfusion engine'
url='https://www.warsow.gg/'
license=('GPL')
arch=('i686' 'x86_64')
depends=('warsow-data')
makedepends=('gendesk' 'cmake' 'libjpeg' 'sdl2' 'libvorbis' 'libtheora' 'openal' 'imagemagick')
conflicts=('warsow')
source=('warsow.launcher'
        'wsw-server.launcher'
        'wswtv-server.launcher'
        'warsow.sysusers'
        'warsow-server.install'
        'warsow-server.service'
        "http://mirror.null.one/warsow_${_pkgver}_sdk.tar.gz")
sha384sums=('6e29b4a6cc3c69a192fc07f738d95848065c68651e4ef18da9aa768f4a62174b4d29d8f3254920f8049e9d6b388b4a9f'
            '8ca7ca88323212b0ee28e6b063f8177b6d7b1609bda0f53cff7aacd57143034d25f2237afec994ee46eeab1ebd20be91'
            'd7199a3163c666382fb01d546b8cd335e7f4e7e05d79467bc2678c412efe643552b17a830accd4985d3bd4c6fc19224e'
            '924d0e428c056ed6025730b9f4fd4ee1e2311d15a8edb28d6d78a57a771de18597a7f96635b710cb327601a76be43e21'
            '7e71719ec5b2b87bed77068ba0468bd2fed8723b02b8e1ae0d91d889db9b7c578c14bfd7dfc1843543d987f3f92e9147'
            '663836285e8e6fe4505cc3cd643e511a54f44cc60e85c94cd8e1e2a60203bbf3814d99678c8431b2c23c705a13553920'
            '53f8ed8611732a7b6a84684a64e60a7224e519b1e9be27780bfa5da3b0d5b8a813733a8305b9a80e0321c14418c7eca9')

prepare() {
  gendesk -n -f --pkgname 'warsow' --pkgdesc "${pkgdesc}" --name 'Warsow' --categories 'Game;ActionGame'
}

build() {
   cd "$srcdir/warsow_${_pkgver}_sdk/source/source"
   cmake -DQFUSION_GAME=Warsow .
   make
}

package_warsow-client() {
   depends+=('sdl2')

   local builddir="$srcdir/warsow_${_pkgver}_sdk/source/source/build"

   # Install binary to /opt/warsow
   install -Dm0755 "$builddir/warsow.$CARCH" "$pkgdir/opt/warsow/warsow"

   # Install launcher to /usr/bin
   install -Dm0755 "$srcdir/warsow.launcher" "$pkgdir/usr/bin/warsow"

   # Install the menu entry
   install -Dm0644 "$srcdir/warsow.desktop" "$pkgdir/usr/share/applications/warsow.desktop"

   # Install the launcher icon
   convert "$srcdir/warsow_${_pkgver}_sdk/source/icons/warsow256x256.xpm" "$srcdir/warsow.png"
   install -Dm0644 "$srcdir/warsow.png" "$pkgdir/usr/share/pixmaps/warsow.png"
}

package_warsow-server() {
   install=warsow-server.install

   local builddir="$srcdir/warsow_${_pkgver}_sdk/source/source/build"

   # Install server libs
   install -D "$builddir/libs/libangelwrap_x86_64.so" "$pkgdir/opt/warsow/libs/libangelwrap_x86_64.so"

   # Install binaries to /opt/warsow
   install -Dm0775 "$builddir/wsw_server.$CARCH" "$pkgdir/opt/warsow/wsw_server"
   install -Dm0755 "$builddir/wswtv_server.$CARCH" "$pkgdir/opt/warsow/wswtv_server"

   # Install launchers to /usr/bin
   install -Dm0755 "$srcdir/wsw-server.launcher" "$pkgdir/usr/bin/wsw-server"
   install -Dm0755 "$srcdir/wswtv-server.launcher" "$pkgdir/usr/bin/wswtv-server"

   # Install sysusers.d config
   install -Dm644 warsow.sysusers "$pkgdir/usr/lib/sysusers.d/warsow.conf"

   # Install systemd service unit
   install -Dm644 warsow-server.service "$pkgdir/usr/lib/systemd/system/warsow-server.service"
}
