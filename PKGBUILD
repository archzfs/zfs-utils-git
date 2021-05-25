# Maintainer: Jan Houben <jan@nexttrex.de>
# Contributor: Jesus Alvarez <jeezusjr at gmail dot com>
#
# This PKGBUILD was generated by the archzfs build scripts located at
#
# http://github.com/archzfs/archzfs
#
pkgname="zfs-utils-git"
_commit='0989d798fa347b87a34607d1fd7b94a1121cf2ad'

pkgver=2021.05.24.r6843.g0989d798f
pkgrel=1
pkgdesc="Kernel module support files for the Zettabyte File System."
makedepends=("python" "python-setuptools" "python-cffi" "git")
optdepends=("python: pyzfs and extra utilities", "python-cffi: pyzfs")
arch=("x86_64")
url="http://zfsonlinux.org/"
source=("git+https://github.com/zfsonlinux/zfs.git#commit=${_commit}"
        "zfs-utils.initcpio.install"
        "zfs-utils.initcpio.hook"
        "zfs-utils.initcpio.zfsencryptssh.install")
sha256sums=("SKIP"
            "29a8a6d76fff01b71ef1990526785405d9c9410bdea417b08b56107210d00b10"
            "ad3e7244aca20fce005860c5118d46a77a0b4f5644d73e9648ea3ba5ff87c4c3"
            "93e6ac4e16f6b38b2fa397a63327bcf7001111e3a58eb5fb97c888098c932a51")
license=("CDDL")
groups=("archzfs-linux-git")
provides=("zfs-utils" "spl-utils")
install=zfs-utils.install
conflicts=("zfs-utils" "spl-utils")
replaces=("spl-utils-common-git" "zfs-utils-common-git")
backup=('etc/zfs/zed.d/zed.rc' 'etc/default/zfs' 'etc/modules-load.d/zfs.conf' 'etc/sudoers.d/zfs')

build() {
    cd "${srcdir}/zfs"
    ./autogen.sh
    ./configure --prefix=/usr --sysconfdir=/etc --sbindir=/usr/bin --with-mounthelperdir=/usr/bin \
                --libdir=/usr/lib --datadir=/usr/share --includedir=/usr/include \
                --with-udevdir=/usr/lib/udev --libexecdir=/usr/lib \
                --with-config=user --enable-systemd --enable-pyzfs
    make
}

package() {
    cd "${srcdir}/zfs"
    make DESTDIR="${pkgdir}" install
    # Remove uneeded files
    rm -r "${pkgdir}"/etc/init.d
    rm -r "${pkgdir}"/usr/share/initramfs-tools
    rm -r "${pkgdir}"/usr/lib/modules-load.d
    # Autoload the zfs module at boot
    mkdir -p "${pkgdir}/etc/modules-load.d"
    printf "%s\n" "zfs" > "${pkgdir}/etc/modules-load.d/zfs.conf"
    # fix permissions
    chmod 750 ${pkgdir}/etc/sudoers.d
    chmod 440 ${pkgdir}/etc/sudoers.d/zfs
    # Install the support files
    install -D -m644 "${srcdir}"/zfs-utils.initcpio.hook "${pkgdir}"/usr/lib/initcpio/hooks/zfs
    install -D -m644 "${srcdir}"/zfs-utils.initcpio.install "${pkgdir}"/usr/lib/initcpio/install/zfs
    install -D -m644 "${srcdir}"/zfs-utils.initcpio.zfsencryptssh.install "${pkgdir}"/usr/lib/initcpio/install/zfsencryptssh
    install -D -m644 contrib/bash_completion.d/zfs "${pkgdir}"/usr/share/bash-completion/completions/zfs
}
