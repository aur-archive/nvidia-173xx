# Contributor : Thomas Baechler <thomas at archlinux dot org>
# Maintainer : Natrio <natrio dog list point ru>

pkgname=nvidia-173xx
pkgver=173.14.39
_kbr="3.15"
_kmin=$_kbr".5"
_kless="3.16"
_extramodules=extramodules-${_kbr}-ARCH
_kernver="$(cat /usr/lib/modules/${_extramodules}/version)"
pkgrel=6
pkgdesc="NVIDIA drivers for linux, 173xx branch."
arch=('i686' 'x86_64')
[ "$CARCH" = "i686"   ] && ARCH=x86
[ "$CARCH" = "x86_64" ] && ARCH=x86_64
url="http://www.nvidia.com/"
depends=("linux>=$_kmin" "linux<$_kless" "nvidia-173xx-utils")
makedepends=("linux-headers>=$_kmin" "linux-headers<$_kless")
conflicts=('nvidia-96xx' 'nvidia-304xx' 'nvidia')
license=('custom')
install=nvidia.install
source=("http://us.download.nvidia.com/XFree86/Linux-$ARCH/${pkgver}/NVIDIA-Linux-$ARCH-${pkgver}-pkg0.run" "linux-3.14.patch")
options=(!strip)
md5sums=('5b423543428554ef33a200fbbe3cb9fc' '6dfb34d8fdf35c1637932f95d2216c46')
[ "$CARCH" = "x86_64" ] && md5sums[0]='0799f194869e40141c7bac8a71762db6'

build() {
 cd $srcdir
 sh NVIDIA-Linux-$ARCH-${pkgver}-pkg0.run --extract-only
 cd NVIDIA-Linux-$ARCH-${pkgver}-pkg0/usr/src/nv/
 patch -p1 -i $srcdir/linux-3.14.patch || return 1
 ln -s Makefile.kbuild Makefile
 make SYSSRC=/usr/lib/modules/${_kernver}/build/ module
}

package() {
 cd $srcdir/NVIDIA-Linux-$ARCH-${pkgver}-pkg0/usr/src/nv/
 mkdir -p $pkgdir/usr/lib/modules/${_extramodules}/
 install -m644 nvidia.ko $pkgdir/usr/lib/modules/${_extramodules}/
 mkdir -p $pkgdir/etc/modprobe.d
 echo "blacklist nouveau" >> $pkgdir/etc/modprobe.d/nouveau_blacklist.conf
 sed -i -e "s/EXTRAMODULES='.*'/EXTRAMODULES='${_extramodules}'/" $startdir/nvidia.install
 gzip "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia.ko"
}
