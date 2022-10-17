# Maintainer: Michael Hansen <zrax0111 gmail com>
# Contributor: Raphaël Doursenaud <rdoursenaud@gpcsolutions.fr>
# Contributor: Jesse Jaara <gmail.com: jesse.jaara>
# Contributor: Tani Aura https://github.com/taniwha3/clion-aur

# Uncomment if you want to disable compressing the package to save some time.
#PKGEXT=.pkg.tar

pkgbase=clion
pkgname=(clion clion-jre clion-cmake clion-gdb clion-lldb)
_pkgname=clion
_dlname=CLion
pkgver=2022.2.4
pkgrel=1
epoch=1
jbr_ver=17.0.4.1
jbr_build=aarch64-b646
jbr_minor=8
pkgdesc="C/C++ IDE. Free 30-day trial."
arch=('x86_64' 'aarch64')
options=(!strip)
url="http://www.jetbrains.com/${_pkgname}"
license=('custom')
makedepends=('rsync')
source=("https://download.jetbrains.com/cpp/${_dlname}-${pkgver}.tar.gz"
        "jetbrains-${pkgbase}.desktop")
source_aarch64=("https://cache-redirector.jetbrains.com/intellij-jbr/jbr-${jbr_ver}-linux-${jbr_build}.${jbr_minor}.tar.gz" "https://github.com/JetBrains/intellij-community/raw/master/bin/linux/aarch64/fsnotifier")
sha256sums=('d88794c698d7bf4d970ba102b85166d5f8c3cb08c4ed5b4cbc150bb505320fab'
            '13c9e7c7f6ef57ee573d133bf30a599390a99087a1f578caea62020e0f742587')
sha256sums_aarch64=('4665ae3f2f6da44493d421f1e943e220de38a43a73a4f3a479b64de0d6aad9d2'
                    'eb3c61973d34f051dcd3a9ae628a6ee37cd2b24a1394673bb28421a6f39dae29')

noextract=("${_dlname}-${pkgver}.tar.gz")

build() {
    rm -rf "${srcdir}/opt"
    mkdir -p "${srcdir}/opt/${pkgbase}"
    bsdtar --strip-components 1 -xf "${_dlname}-${pkgver}.tar.gz" \
           -C "${srcdir}/opt/${pkgbase}"


    # https://youtrack.jetbrains.com/articles/IDEA-A-48/JetBrains-IDEs-on-AArch64#linux
    if [ "${CARCH}" == "aarch64" ]; then
        cd "${srcdir}"
        cp -a fsnotifier opt/${pkgbase}/bin/fsnotifier
        chmod +x opt/${pkgbase}/bin/fsnotifier
        rm -r opt/${pkgbase}/jbr
        cp -a jbr-${jbr_ver}-linux-${jbr_build}.${jbr_minor} opt/${pkgbase}/jbr
        cd ../
    fi
}

package_clion() {
    depends=('libdbusmenu-glib')
    optdepends=(
        'clion-jre: JetBrains custom Java Runtime (Recommended)'
        'clion-cmake: JetBrains packaged CMake tools'
        'clion-gdb: JetBrains packaged GNU debugger'
        'clion-lldb: JetBrains packaged LLVM debugger'
        'java-runtime: JRE - Required if clion-jre is not installed'
        'cmake: Build system - Required if clion-cmake is not installed'
        'gdb: native GNU debugger'
        'lldb: native LLVM debugger'
        'gcc: GNU compiler'
        'clang: LLVM compiler'
        'biicode: C/C++ dependency manager'
        'gtest: C++ testing'
        'swift-language: Swift programming language support (Also requires the plugin)'
        'python: Python 3 programming language support'
        'python2: Python 2 programming language support'
        'doxygen: Code documentation generation'
    )
    backup=("opt/${pkgbase}/bin/clion64.vmoptions"
            "opt/${pkgbase}/bin/idea.properties")

    rsync -rtl "${srcdir}/opt" "${pkgdir}" \
          --exclude=/opt/${pkgbase}/jbr \
          --exclude=/opt/${pkgbase}/bin/cmake \
          --exclude=/opt/${pkgbase}/bin/gdb \
          --exclude=/opt/${pkgbase}/bin/lldb

    mkdir -p "${pkgdir}/usr/bin/"
    mkdir -p "${pkgdir}/usr/share/applications/"
    mkdir -p "${pkgdir}/usr/share/pixmaps/"
    mkdir -p "${pkgdir}/usr/share/licenses/${pkgbase}"

    install -m 644 "${srcdir}/jetbrains-${pkgbase}.desktop" \
            "${pkgdir}/usr/share/applications/"

    ln -s "/opt/${pkgbase}/bin/${_pkgname}.svg" \
            "${pkgdir}/usr/share/pixmaps/${pkgbase}.svg"
    ln -s "/opt/${pkgbase}/license/CLion_Preview_License.txt" \
            "${pkgdir}/usr/share/licenses/${pkgbase}"
    ln -s "/opt/${pkgbase}/bin/${_pkgname}.sh" \
            "${pkgdir}/usr/bin/${pkgbase}"
}

package_clion-jre() {
    install -d -m755 "${pkgdir}/opt/${pkgbase}"
    rsync -rtl "${srcdir}/opt/${pkgbase}/jbr" "${pkgdir}/opt/${pkgbase}"
}

package_clion-cmake() {
    install -d -m755 "${pkgdir}/opt/${pkgbase}/bin"
    rsync -rtl "${srcdir}/opt/${pkgbase}/bin/cmake" "${pkgdir}/opt/${pkgbase}/bin"
}

package_clion-gdb() {
    install -d -m755 "${pkgdir}/opt/${pkgbase}/bin"
    rsync -rtl "${srcdir}/opt/${pkgbase}/bin/gdb" "${pkgdir}/opt/${pkgbase}/bin"
}

package_clion-lldb() {
    install -d -m755 "${pkgdir}/opt/${pkgbase}/bin"
    rsync -rtl "${srcdir}/opt/${pkgbase}/bin/lldb" "${pkgdir}/opt/${pkgbase}/bin"
}
