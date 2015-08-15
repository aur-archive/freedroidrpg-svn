# Contributor: M Rawash <mrawash _stange_curved_character_ gmail D0T com>
# Maintainer: matthiaskrgr <matthiaskrgr _strange_curverd_character_ freedroid D0T org>
pkgname=freedroidrpg-svn
pkgver=5377
pkgrel=1
pkgdesc="A science fiction role playing game which tells the story of a world destroyed by a conflict between robots and their human masters - development version"
url="http://www.freedroid.org"
license=('GPL')
arch=('i686' 'x86_64')
makedepends=('subversion' 'python' 'rsync')
depends=('lua' 'sdl' 'sdl_image' 'libgl' 'sdl_mixer' 'sdl_gfx' 'libogg' 'libvorbis')
#optdepends=('espeak: used to generate the bot voice samples, actually not needed at build or runtime, thus commented')
optdepends=('mesa: to run the game in opengl mode')
conflicts=('freedroidrpg')
replaces=('freedroidrpg')
changelog=changelog
source=("http://buildbot.freedroid.org/logo.png"
        "freedroidrpg.desktop"
        "changelog")
sha1sums=('4a9b3ffadae7367c0f30025370cb7d27ba38fcc1'
          '463708161658e15aa555de14ef18ea1fcc0a2758'
          'e3d9a0316c9b756ad332e4bf627e56048fba6bcd')

_svntrunk="https://freedroid.svn.sourceforge.net/svnroot/freedroid"
_svnmod="freedroid"


build() {
	cd "$srcdir"
	msg "Connecting to SVN server...."

	if [ -d $srcdir/$_svnmod/.svn ]; then
		cd $srcdir/$_svnmod
		svn up
	else
		svn co $_svntrunk --config-dir ./ $_svnmod
	fi
	msg "SVN checkout done or server timeout"

	msg "Removing old build-data, if there is any."
	rm -rf "$srcdir/$_svnmod-build"
	msg "Copying new data into new build-directory via rsync."
	rsync -aC "$srcdir/$_svnmod/" "$srcdir/$_svnmod-build/"
	# note: the rsync command works like 'cp -r "$srcdir/$_svnmod" "$srcdir/$_svnmod-build"' but skips the .svn directory while copying
	cd "$srcdir/$_svnmod-build"

	msg "Running autogen.sh..."
	./autogen.sh
	msg "Running configure..."
	./configure --prefix=/usr
	msg "Compiling..."
	make
}

package() {
	cd "$srcdir/$_svnmod-build"
	msg "Compiling and installing to pkgdir this time..."
	make install DESTDIR=$pkgdir
	msg "Compiling done."

	install -Dm644 $srcdir/logo.png $pkgdir/usr/share/pixmaps/freedroidrpg.png
	install -Dm644 $srcdir/freedroidrpg.desktop $pkgdir/usr/share/applications/freedroidrpg.desktop
}
