pkgname=sioyek-build-src
pkgver=2.0.0.r111.g779b52e
pkgrel=1
epoch=1
pkgdesc="PDF viewer for research papers and technical books."
arch=(x86_64)
license=(GPL3)
url="https://github.com/ahrm/sioyek"
depends=(qt5-base harfbuzz gumbo-parser openjpeg2 jbig2dec libxrandr libjpeg-turbo)
makedepends=(git qt5-3d mujs libxi glu)
provides=(sioyek)
conflicts=(sioyek sioyek-git sioyek-appimage)
source=("git+https://github.com/ahrm/sioyek.git")
sha256sums=('SKIP')

pkgver() {
    cd "sioyek"
    git describe --long --tags | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
    cd "sioyek"
    git submodule update --init --recursive
}

build() {
    cd "sioyek/mupdf"
    make USE_SYSTEM_HARFBUZZ=yes
    cd ..

    qmake-qt5 CONFIG+=linux_app_image DEFINES+=LINUX_STANDARD_PATHS pdf_viewer_build_config.pro
    make
}

package() {
    cd "sioyek"
    make INSTALL_ROOT="${pkgdir}/" install
    install -D tutorial.pdf -t "${pkgdir}/usr/share/sioyek"
    install -d "${pkgdir}/usr/share/sioyek/shaders"
    cp -r pdf_viewer/shaders/* "${pkgdir}/usr/share/sioyek/shaders"
    install -Dm644 -t "${pkgdir}/etc/sioyek" pdf_viewer/keys.config pdf_viewer/prefs.config
}

