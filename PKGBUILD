#_                   _ _ _  _ _____ _  _
#| | _______   ____ _| | | || |___  | || |
#| |/ / _ \ \ / / _` | | | || |_ / /| || |_
#|   <  __/\ V / (_| | | |__   _/ / |__   _|
#|_|\_\___| \_/ \__,_|_|_|  |_|/_/     |_|

#Maintainer: kevall474 <kevall474@tuta.io> <https://github.com/kevall474>
#Credits: Stéphane Gaudreault <stephane@archlinux.org>
#Credits: Sylvain HENRY <hsyl20@yahoo.fr>

#######################################

#Set '1' to build with GCC
#Set '2' to build with CLANG
#Default is empty. It will build with GCC. To build with different compiler just use : env _compiler=(1 or 2) makepkg -s
if [ -z ${_compiler+x} ]; then
  _compiler=
fi

#######################################

pkgname=opencl-headers-git
pkgdesc='OpenCL (Open Computing Language) header files'
pkgver=2021.04.29
pkgrel=1
arch=('x86_64')
url='https://github.com/KhronosGroup/OpenCL-Headers'
license=(Apache-2.0)
source=('OpenCL-Headers::git+https://github.com/KhronosGroup/OpenCL-Headers.git')
md5sums=('SKIP')
makedends=('git' 'cmake' 'make' 'ninja' 'clang' 'llvm' 'llvm-libs' 'gcc' 'gcc-libs')
conflicts=('opencl-headers')
provides=('opencl-headers' 'opencl-headers-git')
optdepends=('opencl-clhpp: C++ support')

pkgver(){
  cd OpenCL-Headers
  echo 2021.04.29.$(date -I | sed 's/-/_/' | sed 's/-/_/').$(git rev-list --count HEAD).$(git rev-parse --short HEAD)
}

build(){
if [[ $_compiler = "1" ]]; then
  export CC="gcc"
  export CXX="g++"
elif [[ $_compiler = "2" ]]; then
  export CC="clang"
  export CXX="clang++"
else
  export CC="gcc"
  export CXX="g++"
fi

  cd OpenCL-Headers

  rm -rf build

  cmake -H. -G Ninja -Bbuild \
  -DCMAKE_C_FLAGS=-m64 \
  -DCMAKE_CXX_FLAGS=-m64 \
  -DCMAKE_INSTALL_PREFIX=/usr

  ninja -C build
}

package() {
  DESTDIR="$pkgdir" ninja -C OpenCL-Headers/build/ install

  # install licence
  install -Dm644 "${srcdir}"/OpenCL-Headers/LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
