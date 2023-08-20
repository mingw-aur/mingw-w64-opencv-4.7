# Maintainer: Pellegrino Prevete <pellegrinoprevete@gmail.com>
# Contributor: Alexey Pavlov <alexpux@gmail.com>

_enable_gst=yes
_realname=opencv
pkgbase=mingw-w64-${_realname}
pkgname=(
  "${MINGW_PACKAGE_PREFIX}-${_realname}-4.7"
  "${MINGW_PACKAGE_PREFIX}-python-${_realname}-4.7"
)
pkgver=4.7.0
pkgrel=4
pkgdesc="Open Source Computer Vision Library (mingw-w64)"
arch=('any')
mingw_arch=('mingw32' 'mingw64' 'ucrt64' 'clang64' 'clang32' 'clangarm64')
url="https://opencv.org/"
license=("spdx:Apache-2.0")
depends=(
  $([[ "$_enable_gst" == "yes" ]] && \
      echo "${MINGW_PACKAGE_PREFIX}-gst-plugins-base")
  "${MINGW_PACKAGE_PREFIX}-ceres-solver"
  "${MINGW_PACKAGE_PREFIX}-ffmpeg"
  "${MINGW_PACKAGE_PREFIX}-freetype"
  "${MINGW_PACKAGE_PREFIX}-gflags"
  "${MINGW_PACKAGE_PREFIX}-glog"
  "${MINGW_PACKAGE_PREFIX}-harfbuzz"
  "${MINGW_PACKAGE_PREFIX}-hdf5"
  "${MINGW_PACKAGE_PREFIX}-tbb"
  "${MINGW_PACKAGE_PREFIX}-jasper"
  "${MINGW_PACKAGE_PREFIX}-libiconv"
  "${MINGW_PACKAGE_PREFIX}-libjpeg"
  "${MINGW_PACKAGE_PREFIX}-libpng"
  "${MINGW_PACKAGE_PREFIX}-libtiff"
  "${MINGW_PACKAGE_PREFIX}-libwebp"
  "${MINGW_PACKAGE_PREFIX}-ogre3d"
  "${MINGW_PACKAGE_PREFIX}-openblas"
  "${MINGW_PACKAGE_PREFIX}-openjpeg2"
  "${MINGW_PACKAGE_PREFIX}-openexr"
  "${MINGW_PACKAGE_PREFIX}-protobuf"
  "${MINGW_PACKAGE_PREFIX}-tesseract-ocr"
  "${MINGW_PACKAGE_PREFIX}-zlib"
  # "${MINGW_PACKAGE_PREFIX}-gtkglext"
)
makedepends=(
  "${MINGW_PACKAGE_PREFIX}-cc"
  "${MINGW_PACKAGE_PREFIX}-cmake"
  "${MINGW_PACKAGE_PREFIX}-ninja"
  "${MINGW_PACKAGE_PREFIX}-cereal"
  "${MINGW_PACKAGE_PREFIX}-python-numpy"
  "${MINGW_PACKAGE_PREFIX}-python-flake8"
  "${MINGW_PACKAGE_PREFIX}-python-pylint"
  $([[ ${CARCH} != i686 ]] && \
      echo "${MINGW_PACKAGE_PREFIX}-qt6-base" \
           "${MINGW_PACKAGE_PREFIX}-qt6-5compat" || \
      echo "${MINGW_PACKAGE_PREFIX}-qt5-base")
  "${MINGW_PACKAGE_PREFIX}-tiny-dnn"
  "${MINGW_PACKAGE_PREFIX}-vtk"
  "${MINGW_PACKAGE_PREFIX}-nlohmann-json"
  "${MINGW_PACKAGE_PREFIX}-utf8cpp"
  "${MINGW_PACKAGE_PREFIX}-eigen3"
)
optdepends=(
  "${MINGW_PACKAGE_PREFIX}-vtk: opencv_viz module")
if [[ ${CARCH} != i686 ]]; then
  optdepends+=(
    "${MINGW_PACKAGE_PREFIX}-qt6-5compat: for the HighGUI module")
else
  optdepends+=(
    "${MINGW_PACKAGE_PREFIX}-qt5-base: for the HighGUI module")
fi
provides=(
  "${MINGW_PACKAGE_PREFIX}-${_realname}=${pkgver}"
)
provides=(
  "${MINGW_PACKAGE_PREFIX}-${_realname}"
)
_url="https://github.com/${_realname}"
source=(
  "${_realname}-${pkgver}.tar.gz::${_url}/${_realname}/archive/${pkgver}.tar.gz"
  "${_realname}_contrib-${pkgver}.tar.gz::${_url}/opencv_contrib/archive/${pkgver}.tar.gz"
  '0001-mingw-w64-cmake.patch'
  '0002-find-protobuf-config.patch'
  '0004-generate-proper-pkg-config-file.patch'
  '0008-mingw-w64-cmake-lib-path.patch'
  '0014-python-install-path.patch'
  '0015-windres-cant-handle-spaces-in-defines.patch'
  '0021-requires-vtk.patch'
  '0104-rgbd-module-missing-include.patch'
  '0106-use-find-package-with-hdf5.patch')
sha256sums=(
  '8df0079cdbe179748a18d44731af62a245a45ebf5085223dc03133954c662973'
  '42df840cf9055e59d0e22c249cfb19f04743e1bdad113d31b1573d3934d62584'
  '34e63a897024d41adeadcf593480ae4074ecaed5fc7b05ba5cc2469c7669a83e'
  '72ce1660e1d654946ae3d4144348133fb39b4044afe27efee7f8ee8f7c16a5fe'
  '7fac6a7788638f8843f562381413ce13c59038d2fafc5dc05258195128e5caf5'
  '7398e66f80be37382bd427b5eb3a1201a23113c14e71435a44df8779ea1b8a34'
  '0c310a580d6700601d4b4f824b849c0f0d64d3953e249f04c6a91f15aa8bee0a'
  '11522ffedb22980ba8d09ff715a25327fe14806e55588bffa761e39e1d035ea5'
  'f572f94eb3e438eb1e0700ee04b9c999cd3216b39ba04321148137cecf6bbcb5'
  'c6c92cf39dfe45b8fb41d80ac0de3cd304e8b695420b307fd4320a105d8fe9f4'
  '01db42dc6fec466433ebe701be4fa21d5cb439cc156b04d969290a98e18f7467')

# Helper macros to help make tasks easier #
apply_patch_with_msg() {
  for _patch in "$@"
  do
    msg2 "Applying ${_patch}"
    patch -Nbp1 -i "${srcdir}/${_patch}"
  done
}

del_file_exists() {
  for _fname in "$@"
  do
    if [ -f ${_fname} ]; then
      rm -rf ${_fname}
    fi
  done
}
# =========================================== #

prepare() {
  cd "${srcdir}/${_realname}-${pkgver}"
  apply_patch_with_msg \
    0001-mingw-w64-cmake.patch \
    0002-find-protobuf-config.patch \
    0004-generate-proper-pkg-config-file.patch \
    0008-mingw-w64-cmake-lib-path.patch \
    0014-python-install-path.patch \
    0015-windres-cant-handle-spaces-in-defines.patch \
    0021-requires-vtk.patch

  cd "${srcdir}/${_realname}_contrib-${pkgver}"
  apply_patch_with_msg \
    0104-rgbd-module-missing-include.patch \
    0106-use-find-package-with-hdf5.patch
}

build() {
  mkdir -p ${srcdir}/build-${MSYSTEM} && cd ${srcdir}/build-${MSYSTEM}

  export OpenEXR_HOME=${MINGW_PREFIX}
  export OpenBLAS_HOME=${MINGW_PREFIX}

  local -a extra_config
  if check_option "debug" "n"; then
    extra_config+=("-DCMAKE_BUILD_TYPE=Release")
  else
    extra_config+=("-DCMAKE_BUILD_TYPE=Debug")
  fi

  if [ ${MSYSTEM} == "CLANG64" ]; then
    extra_config+=("-DCPU_DISPATCH=SSE4_1;SSE4_2;AVX;FP16;AVX2")
  fi

  CFLAGS+=" -D_POSIX_SOURCE -Wno-attributes" \
  CXXFLAGS+=" -D_POSIX_SOURCE -Wno-attributes" \
  MSYS2_ARG_CONV_EXCL="-DCMAKE_INSTALL_PREFIX=;-DPYTHON2_PACKAGES_PATH=;-DPYTHON3_PACKAGES_PATH=" \
  ${MINGW_PREFIX}/bin/cmake.exe -Wno-dev \
    -G"Ninja" \
    -DCMAKE_INSTALL_PREFIX=${MINGW_PREFIX} \
    -DCMAKE_SKIP_RPATH=ON \
    -DCMAKE_SHARED_LIBRARY_NAME_WITH_VERSION=ON \
    -DCMAKE_CXX_STANDARD=17 \
    -DINSTALL_C_EXAMPLES=OFF \
    -DINSTALL_PYTHON_EXAMPLES=OFF \
    -DBUILD_WITH_DEBUG_INFO=OFF \
    -DBUILD_DOCS=OFF \
    -DBUILD_TESTS=OFF \
    -DBUILD_PERF_TESTS=OFF \
    -DBUILD_PROTOBUF=OFF \
    -DBUILD_EXAMPLES=OFF \
    -DBUILD_JAVA=OFF \
    -DBUILD_opencv_python3=ON \
    -DBUILD_opencv_bioinspired=OFF \
    -DBUILD_opencv_python2=OFF \
    -DWITH_FFMPEG=ON \
    -DWITH_FREETYPE=ON \
    -DWITH_OPENCL=ON \
    -DWITH_OPENGL=ON \
    -DWITH_OPENJPEG=ON \
    -DWITH_QT=ON \
    -DWITH_TBB=ON \
    -DWITH_VTK=ON \
    -DWITH_GDAL=OFF \
    -DWITH_GSTREAMER=$([[ "$_enable_gst" == "yes" ]] && echo "ON" || echo "OFF") \
    -DWITH_GTK=OFF \
    -DWITH_MSMF=OFF \
    -DWITH_OBSENSOR=OFF \
    -DWITH_XINE=OFF \
    -DOPENCV_EXTRA_MODULES_PATH=../${_realname}_contrib-${pkgver}/modules \
    -DOPENCV_PYTHON3_VERSION=$(${MINGW_PREFIX}/bin/python -c "import sys;sys.stdout.write('.'.join(map(str, sys.version_info[:2])))") \
    -DOPENCV_SKIP_PYTHON_LOADER=ON \
    -DOPENCV_GENERATE_PKGCONFIG=ON \
    -DOPENCV_PYTHON_INSTALL_PATH=lib \
    -DOPENCV_SKIP_CMAKE_ROOT_CONFIG=ON \
    -DOPENCV_FFMPEG_USE_FIND_PACKAGE=ON \
    -DOPENCV_FFMPEG_SKIP_DOWNLOAD=ON \
    -DOPENCV_FORCE_VTK=ON \
    -DOPENCV_DLLVERSION="" \
    -DPYTHON_DEFAULT_EXECUTABLE=${MINGW_PREFIX}/bin/python \
    -DPYTHON3_EXECUTABLE=${MINGW_PREFIX}/bin/python \
    -DPYTHON3_PACKAGES_PATH=$(cygpath -u $(${MINGW_PREFIX}/bin/python -c "import sysconfig; print(sysconfig.get_path('purelib'))")) \
    -DPROTOBUF_UPDATE_FILES=ON \
    -Dprotobuf_MODULE_COMPATIBLE=ON \
    -DENABLE_PRECOMPILED_HEADERS=OFF \
    "${extra_config[@]}" \
    ../${_realname}-${pkgver}

  ${MINGW_PREFIX}/bin/cmake --build .
}

package_opencv-4.7() {
  cd "${srcdir}/build-${MSYSTEM}"
  DESTDIR="${pkgdir}" ${MINGW_PREFIX}/bin/cmake --install .

  # Split Python bindings
  rm -r "${pkgdir}${MINGW_PREFIX}"/lib/python3*
}

package_python-opencv-4.7() {
  pkgdesc='Python bindings for OpenCV (mingw-w64)'
  depends=(
    "${MINGW_PACKAGE_PREFIX}-opencv"
    "${MINGW_PACKAGE_PREFIX}-python-numpy"
    $([[ ${CARCH} != i686 ]] && echo \
      "${MINGW_PACKAGE_PREFIX}-qt6-base" \
      "${MINGW_PACKAGE_PREFIX}-qt6-5compat" || echo \
      "${MINGW_PACKAGE_PREFIX}-qt5-base")
    "${MINGW_PACKAGE_PREFIX}-vtk")
  unset optdepends
  provides=(
    "${MINGW_PACKAGE_PREFIX}-python-${_realname}=${pkgver}"
  )
  conflicts=(
    "${MINGW_PACKAGE_PREFIX}-python-${_realname}"
  )

  cd "${srcdir}/build-${MSYSTEM}"
  DESTDIR="${pkgdir}" ${MINGW_PREFIX}/bin/cmake --install modules/python3
}

# template start; name=mingw-w64-splitpkg-wrappers; version=1.0;
# vim: set ft=bash :

# generate wrappers
for _name in "${pkgname[@]}"; do
  _short="package_${_name#${MINGW_PACKAGE_PREFIX}-}"
  _func="$(declare -f "${_short}")"
  eval "${_func/#${_short}/package_${_name}}"
done
# template end;
