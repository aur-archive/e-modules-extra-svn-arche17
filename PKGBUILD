# Maintainer: ultraviolet <ultravioletnanokitty@gmail.com>
# Contributor: Ronald van Haren <ronald.archlinux.org>

pkgname=e-modules-extra-svn-arche17
pkgver=latest
pkgrel=1
pkgdesc="Extra gadgets for e17, based on the arche17 project's PKGBUILD"
arch=('i686' 'x86_64')
groups=('e17-extra-svn-arche17')
url="http://www.enlightenment.org"
license=('BSD')
depends=('e-svn' 'libxkbfile' 'libmpd' 'efreet-svn' 'e_dbus-svn' 'emprint-svn')
makedepends=('subversion')
conflicts=('e-modules-extra' 'e-modules-extra-svn')
provides=('e-modules-extra' 'e-modules-extra-svn')
options=('!libtool')
source=()
md5sums=()

_svntrunk="http://svn.enlightenment.org/svn/e/trunk/E-MODULES-EXTRA"
_svnmod="E-MODULES-EXTRA"

build() {
   cd $srcdir

  msg "Connecting to $_svntrunk SVN server...."
  if [ -d $_svnmod/.svn ]; then
    (cd $_svnmod && svn up)
  else
    svn co $_svntrunk --config-dir ./ $_svnmod
  fi

  msg "SVN checkout done or server timeout"
  msg "Starting make..."

  cp -r $_svnmod $_svnmod-build
  cd $_svnmod-build

  # fix build issue
  sed -i 's|efreet/Efreet.h|efreet-0/Efreet.h|' winlist-ng/src/e_mod_main.h || return 1

for i in alarm calendar comp-scale cpu diskio deskshow \
	empris engage eooorg everything-aspell everything-mpris \
	everything-pidgin everything-places everything-shotgun \
	everything-tracker everything-wallpaper everything-websearch  \
	execwatch flame forecasts iiirk itask mail mem \
        moon mpdule net news penguins photo places quickaccess \
	rain screenshot slideshow snow taskbar tclock uptime \
	weather winlist-ng winselector wlan; do

  cd $i
  ./autogen.sh --prefix=/usr
  make
  cd ..
done
} 

package() {
  cd $srcdir/$_svnmod-build

for i in alarm calendar comp-scale cpu diskio deskshow \
        empris engage eooorg everything-aspell everything-mpris \
        everything-pidgin everything-places everything-shotgun \
        everything-tracker everything-wallpaper everything-websearch  \
        execwatch flame forecasts iiirk itask mail mem \
        moon mpdule net news penguins photo places quickaccess \
        rain screenshot slideshow snow taskbar tclock uptime \
        weather winlist-ng winselector wlan; do

  cd $i
  make DESTDIR=$pkgdir install

# install license files
  if [ -e $srcdir/$_svnmod-build/$i/COPYING ]; then
    install -Dm644 $srcdir/$_svnmod-build/$i/COPYING \
        $pkgdir/usr/share/licenses/$pkgname/$i/COPYING
  fi
 
  if [ -e $srcdir/$_svnmod-build/$i/COPYING-PLAIN ]; then
    install -Dm644 $srcdir/$_svnmod-build/$i/COPYING-PLAIN \
        $pkgdir/usr/share/licenses/$pkgname/$i/COPYING-PLAIN
  fi
 
  cd ..
 done
 rm -r $srcdir/$_svnmod-build

}
