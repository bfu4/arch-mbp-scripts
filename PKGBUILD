# Maintainer: Aun-Ali Zaidi <admin@kodeit.net>
# Contributor: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>

pkgbase=linux-mbp
pkgver=5.7.17
_srcname=linux-${pkgver}
pkgrel=1
pkgdesc='Linux for MBP'
_srctag=v${pkgver%.*}-${pkgver##*.}
url="https://git.archlinux.org/linux.git/log/?h=v$_srctag"
arch=(x86_64)
license=(GPL2)
makedepends=(
  bc kmod libelf pahole
  xmlto python-sphinx python-sphinx_rtd_theme graphviz imagemagick
  git
)
options=('!strip')

source=(
  https://www.kernel.org/pub/linux/kernel/v${pkgver//.*}.x/linux-${pkgver}.tar.xz
  https://www.kernel.org/pub/linux/kernel/v${pkgver//.*}.x/linux-${pkgver}.tar.sign
  config         # the main kernel config file

  # patches

  0001-ZEN-Add-sysctl-and-CONFIG-to-disallow-unprivileged-C.patch
  #0001-ZEN-Add-sysctl-and-CONFIG-to-disallow-unprivileged-CLONE_NEWUSER.patch
  #0001-fix-iwlwifi.patch
  #0001-iwlwifi-Make-some-Killer-Wireless-AC-1550-cards-working-again.patch
  0001-lenovo-wmi1.patch
  0001-lenovo-wmi2.patch
  0001-nonupstream-navi10-vfio-reset.patch
  #0001-pci-edr-log-only-ACPI_NOTIFY_DISCONNECT_RECOVER-events.patch
  #0001-virt-vbox-Add-support-for-the-new-VBG_IOCTL_ACQUIRE_GUEST_CAPABILITIES-ioctl.patch
  #0002-PCI-EDR-Log-only-ACPI_NOTIFY_DISCONNECT_RECOVER-even.patch
  #0002-pinctrl-amd.patch
  #0003-virt-vbox-Add-support-for-the-new-VBG_IOCTL_ACQUIRE_.patch
  2001-drm-amd-display-Force-link_rate-as-LINK_RATE_RBR2-fo.patch
  3001-applesmc-convert-static-structures-to-drvdata.patch
  3002-applesmc-make-io-port-base-addr-dynamic.patch
  3003-applesmc-switch-to-acpi_device-from-platform.patch
  3004-applesmc-key-interface-wrappers.patch
  3005-applesmc-basic-mmio-interface-implementation.patch
  3006-applesmc-fan-support-on-T2-Macs.patch
  4001-touchpad.patch
  #4002-keyboard-backlight.patch
  5001-brcmfmac-move-brcmf_mp_device-into-its-own-header.patch
  5002-brcmfmac-Add-ability-to-manually-specify-FW-rambase-.patch
  6001-media-uvcvideo-Add-support-for-Apple-T2-attached-iSi.patch
  aufs5-base.patch
  aufs5-kbuild.patch
  #aufs5-loopback.patch
  aufs5-mmap.patch
  aufs5-standalone.patch
  aufs5.x-rcN-20200622.patch
  tmpfs-idr.patch
  vfs-ino.patch
  wifi.patch
  sphinx-workaround.patch 
)

validpgpkeys=(
  'ABAF11C65A2970B130ABE3C479BE3E4300411886'  # Linus Torvalds
  '647F28654894E3BD457199BE38DBBDC86092693E'  # Greg Kroah-Hartman
)

sha256sums=('cc8c3611c9bdcd29266b2606ab6c3a07a32fb64fd7f1cd731100e4eddb565225'
            'SKIP'
            '17f81d9d03fbaefeafacfe1fe2f737c8f4dfeccfeb037170523021d9a0354ed7'
            '8cb21e0b3411327b627a9dd15b8eb773295a0d2782b1a41b2a8839d1b2f5778c'
            'd831e9dc07901a1ca581afc18a12690fcedc208609a560b6486acfd6b333cf19'
            '72363eb41f7efca92a068f6ba7805a566f58531709c0e3b37357dece562feab6'
            '49363f975a5c5888afad5d2734fb76fe6b49df48a2f49d050f9e50d7dda65289'
            '1c2363d3f577b58c5d6b2b7919b0d77a8615701adc36fdf31d63c46e61c73e01'
            '25e1aac0d44d72e377f08e4f4b90351cffcacc0be63e02a4033cb99f10cc9fe7'
            'c70118659c5cf6a5c7f060c941d46fdd3b1e6d28f2b62c24a941745f2b3c4732'
            '3855aa07fab97d202900216951225b6952d7c716258a3c3727df8e6277289ee0'
            '9e5e0b45fe007ed214049b26b44174ee8f61376076e80fd33bba9fdac001e157'
            '3c8a361370ed3ee094e2c8af1ff5360fd78f24e387c250904031fb70e8f2bb6e'
            '8e43d95104301913737e5d73860f0e21bb0e5e25dcfd0f16d48a0715b38c98a1'
            'e1d72fdb0a7a66a6f3fc8afb6fe056f28cfa088c1cc9c799b93405b62a274b96'
            '6785a219662672430af11fdc650c19bf7d2073ae696247a37635a30f28bcb693'
          #  '0318952f59efdce4dc72703adc764940db6fdff184960c27a23a80c3413d8a60'
            #'e632f2959efca848fd28acb5e278cc476f8fb54d70ca95272b0a76add47e474e'
            'eb5134e6b7415528547120e661aa58d7125cc657e982c924989d7a63d253d85e'
            '17f11a531e975f401449e5a0e230c596cdaff51c95a9e7b70bc7ce9455a1f0e1')

export KBUILD_BUILD_HOST=archlinux
export KBUILD_BUILD_USER=$pkgbase
export KBUILD_BUILD_TIMESTAMP="$(date -Ru${SOURCE_DATE_EPOCH:+d @$SOURCE_DATE_EPOCH})"

prepare() {
  cd $_srcname

  echo "Setting version..."
  scripts/setlocalversion --save-scmversion
  echo "-$pkgrel" > localversion.10-pkgrel
  echo "${pkgbase#linux}" > localversion.20-pkgname

  local src
  for src in "${source[@]}"; do
    src="${src%%::*}"
    src="${src##*/}"
    [[ $src = *.patch ]] || continue
    echo "Applying patch $src..."
    patch -Np1 < "../$src"
  done

  echo "Setting config..."
  cp ../config .config
  make olddefconfig

  make -s kernelrelease > version
  echo "Prepared $pkgbase version $(<version)"
}

build() {
  cd $_srcname
  make all
  make htmldocs
}

_package() {
  pkgdesc="The $pkgdesc kernel and modules"
  depends=(coreutils kmod initramfs)
  optdepends=('crda: to set the correct wireless channels of your country'
              'linux-firmware: firmware images needed for some devices')
  provides=(VIRTUALBOX-GUEST-MODULES WIREGUARD-MODULE)
  replaces=(virtualbox-guest-modules-arch wireguard-arch)
  provides=("linux=$pkgver")

  cd $_srcname
  local kernver="$(<version)"
  local modulesdir="$pkgdir/usr/lib/modules/$kernver"

  echo "Installing boot image..."
  # systemd expects to find the kernel here to allow hibernation
  # https://github.com/systemd/systemd/commit/edda44605f06a41fb86b7ab8128dcf99161d2344
  install -Dm644 "$(make -s image_name)" "$modulesdir/vmlinuz"

  # Used by mkinitcpio to name the kernel
  echo "$pkgbase" | install -Dm644 /dev/stdin "$modulesdir/pkgbase"

  echo "Installing modules..."
  make INSTALL_MOD_PATH="$pkgdir/usr" INSTALL_MOD_STRIP=1 modules_install

  # remove build and source links
  rm "$modulesdir"/{source,build}
}

_package-headers() {
  pkgdesc="Headers and scripts for building modules for the $pkgdesc kernel"
  provides=("linux-headers=$pkgver")

  cd $_srcname
  local builddir="$pkgdir/usr/lib/modules/$(<version)/build"

  echo "Installing build files..."
  install -Dt "$builddir" -m644 .config Makefile Module.symvers System.map \
    localversion.* version vmlinux
  install -Dt "$builddir/kernel" -m644 kernel/Makefile
  install -Dt "$builddir/arch/x86" -m644 arch/x86/Makefile
  cp -t "$builddir" -a scripts

  # add objtool for external module building and enabled VALIDATION_STACK option
  install -Dt "$builddir/tools/objtool" tools/objtool/objtool

  # add xfs and shmem for aufs building
  mkdir -p "$builddir"/{fs/xfs,mm}

  echo "Installing headers..."
  cp -t "$builddir" -a include
  cp -t "$builddir/arch/x86" -a arch/x86/include
  install -Dt "$builddir/arch/x86/kernel" -m644 arch/x86/kernel/asm-offsets.s

  install -Dt "$builddir/drivers/md" -m644 drivers/md/*.h
  install -Dt "$builddir/net/mac80211" -m644 net/mac80211/*.h

  # http://bugs.archlinux.org/task/13146
  install -Dt "$builddir/drivers/media/i2c" -m644 drivers/media/i2c/msp3400-driver.h

  # http://bugs.archlinux.org/task/20402
  install -Dt "$builddir/drivers/media/usb/dvb-usb" -m644 drivers/media/usb/dvb-usb/*.h
  install -Dt "$builddir/drivers/media/dvb-frontends" -m644 drivers/media/dvb-frontends/*.h
  install -Dt "$builddir/drivers/media/tuners" -m644 drivers/media/tuners/*.h

  echo "Installing KConfig files..."
  find . -name 'Kconfig*' -exec install -Dm644 {} "$builddir/{}" \;

  echo "Removing unneeded architectures..."
  local arch
  for arch in "$builddir"/arch/*/; do
    [[ $arch = */x86/ ]] && continue
    echo "Removing $(basename "$arch")"
    rm -r "$arch"
  done

  echo "Removing documentation..."
  rm -r "$builddir/Documentation"

  echo "Removing broken symlinks..."
  find -L "$builddir" -type l -printf 'Removing %P\n' -delete

  echo "Removing loose objects..."
  find "$builddir" -type f -name '*.o' -printf 'Removing %P\n' -delete

  echo "Stripping build tools..."
  local file
  while read -rd '' file; do
    case "$(file -bi "$file")" in
      application/x-sharedlib\;*)      # Libraries (.so)
        strip -v $STRIP_SHARED "$file" ;;
      application/x-archive\;*)        # Libraries (.a)
        strip -v $STRIP_STATIC "$file" ;;
      application/x-executable\;*)     # Binaries
        strip -v $STRIP_BINARIES "$file" ;;
      application/x-pie-executable\;*) # Relocatable binaries
        strip -v $STRIP_SHARED "$file" ;;
    esac
  done < <(find "$builddir" -type f -perm -u+x ! -name vmlinux -print0)

  echo "Stripping vmlinux..."
  strip -v $STRIP_STATIC "$builddir/vmlinux"

  echo "Adding symlink..."
  mkdir -p "$pkgdir/usr/src"
  ln -sr "$builddir" "$pkgdir/usr/src/$pkgbase"
}

_package-docs() {
  pkgdesc="Documentation for the $pkgdesc kernel"

  cd $_srcname
  local builddir="$pkgdir/usr/lib/modules/$(<version)/build"

  echo "Installing documentation..."
  local src dst
  while read -rd '' src; do
    dst="${src#Documentation/}"
    dst="$builddir/Documentation/${dst#output/}"
    install -Dm644 "$src" "$dst"
  done < <(find Documentation -name '.*' -prune -o ! -type d -print0)

  echo "Adding symlink..."
  mkdir -p "$pkgdir/usr/share/doc"
  ln -sr "$builddir/Documentation" "$pkgdir/usr/share/doc/$pkgbase"
}

pkgname=("$pkgbase" "$pkgbase-headers" "$pkgbase-docs")
for _p in "${pkgname[@]}"; do
  eval "package_$_p() {
    $(declare -f "_package${_p#$pkgbase}")
    _package${_p#$pkgbase}
  }"
done

# vim:set ts=8 sts=2 sw=2 et:
