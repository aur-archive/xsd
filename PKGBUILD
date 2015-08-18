# Maintainer: kevku <kevku@gmx.com>
pkgname=xsd
pkgver=3.3.0
pkgrel=5
pkgdesc="An open-source, cross-platform W3C XML Schema to C++ data binding compiler"
arch=('i686' 'x86_64')
url="http://www.codesynthesis.com/products/xsd"
license=('GPL2')
depends=('boost-libs' 'libxsd-frontend' 'libbackend-elements' 'zlib')
makedepends=('build' 'boost')
source=("http://www.codesynthesis.com/download/$pkgname/3.3/$pkgname-$pkgver.tar.bz2"
	"3.3.0-disable_examples_and_tests.patch"
	"3.3.0-fix_include.patch"
	"3.3.0-fix_tests.patch"
	"3.3.0-xsdcxx-rename.patch"
	"xsd-3.3.0-2.patch")

md5sums=('3723c74d320dd1e23afcc07510ed0306'
         'b8545d9d37d31a731c9fea425b3730b0'
         '5e8e993f015bee8fa1f99bc416943031'
         '1f346b4b4a54c7b78b51f2fe81ef8e7f'
         '99df6f9149504acd3276306f7f33367e'
         'c18c3778954c11bf86512a25de491c47')

build() {
    cd $srcdir/$pkgname-$pkgver
    patch -Np1 -i "$srcdir/$pkgver-disable_examples_and_tests.patch"
    patch -Np1 -i "$srcdir/$pkgver-xsdcxx-rename.patch"
    patch -Np1 -i "$srcdir/$pkgver-fix_include.patch"
    patch -Np1 -i "$srcdir/$pkgver-fix_tests.patch"
    patch -Np1 -i "$srcdir/xsd-3.3.0-2.patch"

    mkdir -p \
		build/cxx/gnu \
		build/import/lib{ace,boost,cult,backend-elements,xerces-c,xqilla,xsd-frontend,z}

    cat > build/configuration-dynamic.make <<- EOF
	xsd_with_zlib := y
	xsd_with_ace := n
	xsd_with_xdr := y
	xsd_with_dbxml := n
	xsd_with_xqilla := y
	xsd_with_boost_date_time := y
	xsd_with_boost_serialization := y
		EOF

     cat > build/cxx/configuration-dynamic.make <<- EOF
	cxx_id       := gnu
	cxx_optimize := y
	cxx_debug    := n
	cxx_rpath    := n
	cxx_pp_extra_options :=
	cxx_extra_options    := ${CXXFLAGS} -I/usr/include/boost -lboost_system -lboost_filesystem
	cxx_ld_extra_options := ${LDFLAGS}
	cxx_extra_libs       :=
	cxx_extra_lib_paths  :=
		EOF

     cat > build/cxx/gnu/configuration-dynamic.make <<- EOF
	cxx_gnu := g++
	cxx_gnu_libraries :=
	cxx_gnu_optimization_options :=
		EOF

     cat > build/import/libace/configuration-dynamic.make <<- EOF
	libace_installed := n
		EOF

     cat > build/import/libbackend-elements/configuration-dynamic.make <<- EOF
	libbackend_elements_installed := y
		EOF

     cat > build/import/libboost/configuration-dynamic.make <<- EOF
	libboost_installed := y
	libboost_suffix :=
	libboost_system := y
		EOF

     cat > build/import/libcult/configuration-dynamic.make <<- EOF
	libcult_installed := y
		EOF

     cat > build/import/libxerces-c/configuration-dynamic.make <<- EOF
	libxerces_c_installed := y
		EOF

     cat > build/import/libxqilla/configuration-dynamic.make <<- EOF
	libxqilla_installed := y
		EOF

     cat > build/import/libxsd-frontend/configuration-dynamic.make <<- EOF
	libxsd_frontend_installed := y
		EOF

     cat > build/import/libz/configuration-dynamic.make <<- EOF
	libz_installed := y
		EOF

     make
}

package() {
 cd $srcdir/$pkgname-$pkgver/
    make install_prefix="$pkgdir/usr" install
    # Renaming binary/manpage to avoid collision with mono-2.0's xsd/xsd2
    mv $pkgdir/usr/bin/xsd{,cxx}
    mv $pkgdir/usr/share/man/man1/xsd{,cxx}.1
}
