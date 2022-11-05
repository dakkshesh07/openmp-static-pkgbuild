# Maintainer: Dakkshesh <dakkshesh5@gmail.com>

pkgname=openmp-static
pkgver=$PKG_VER
pkgrel=$PKG_REL
pkgdesc="LLVM OpenMP Runtime Library"
arch=('x86_64')
url="https://openmp.llvm.org/"
license=('custom:Apache 2.0 with LLVM Exception')
depends=('llvm-libs' 'libelf' 'libffi')
makedepends=('llvm' 'clang' 'cmake' 'ninja')
conflicts=(openmp)
replaces=(openmp)
options=('!lto') # https://bugzilla.redhat.com/show_bug.cgi?id=1988155
_source_base=https://github.com/llvm/llvm-project/releases/download/llvmorg-$pkgver
source=($_source_base/openmp-$pkgver.src.tar.xz)
sha256sums=('4f731ff202add030d9d68d4c6daabd91d3aeed9812e6a5b4968815cfdff0eb1f')

prepare() {
  cd openmp-$pkgver.src
  mkdir build
}

build() {
  cd openmp-$pkgver.src/build

  export OPT_FLAGS="-O3 -fgraphite-identity -floop-nest-optimize"
  export CFLAGS=${CFLAGS/-O2/$OPT_FLAGS}
  export CXXFLAGS=${CXXFLAGS/-O2/$OPT_FLAGS}
  export LDFLAGS=${LDFLAGS/-O1/-O3}

  local cmake_args=(
    -G Ninja
    -DCMAKE_BUILD_TYPE=Release
    -DCMAKE_INSTALL_PREFIX=/usr
    -DLIBOMP_ENABLE_SHARED=OFF
    -DLIBOMP_INSTALL_ALIASES=OFF
    -DCMAKE_C_FLAGS="$CFLAGS"
    -DCMAKE_ASM_FLAGS="$CFLAGS"
    -DCMAKE_CXX_FLAGS="$CXXFLAGS"
    -DCMAKE_EXE_LINKER_FLAGS="$LDFLAGS"
    -DCMAKE_MODULE_LINKER_FLAGS="$LDFLAGS"
    -DCMAKE_SHARED_LINKER_FLAGS="$LDFLAGS"
  )
  cmake .. "${cmake_args[@]}"
  ninja -j$(nproc --all)
}

package() {
  cd openmp-$pkgver.src/build

  DESTDIR="$pkgdir" ninja install
  install -Dm644 ../LICENSE.TXT "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  rm "$pkgdir/usr/lib/libarcher_static.a"
}

# vim:set ts=2 sw=2 et:
