pkgdesc="ROS - qt_gui_cpp provides the foundation for C++-bindings for qt_gui and creates bindings for every generator available."
url='https://wiki.ros.org/qt_gui_cpp'

pkgname='ros-melodic-qt-gui-cpp'
pkgver='0.3.16'
arch=('i686' 'x86_64' 'aarch64' 'armv7h' 'armv6h')
pkgrel=2
license=('BSD')

ros_makedepends=(
	ros-melodic-python-qt-binding
	ros-melodic-cmake-modules
	ros-melodic-catkin
	ros-melodic-pluginlib
)

makedepends=(
	'cmake'
	'ros-build-tools'
	${ros_makedepends[@]}
	tinyxml
	qt5-base
	pkg-config
)

ros_depends=(
	ros-melodic-qt-gui
	ros-melodic-pluginlib
)

depends=(
	${ros_depends[@]}
	tinyxml
    sip
    python-sip
)

_dir="qt_gui_core-${pkgver}/qt_gui_cpp"
source=("${pkgname}-${pkgver}.tar.gz"::"https://github.com/ros-visualization/qt_gui_core/archive/${pkgver}.tar.gz"
        "fix-typesystem.patch"::"https://github.com/stertingen/qt_gui_core/commit/f3d0b885d4b9dc074553268ee7701b45240a0bd8.patch")
sha256sums=('efa5ecf7ec22de606b3c0e039f43aacc2f2d79d74d7e17ecceecf2cafd22d128'
            'dfc3a3a8465eecacaf81f3d8ac6b90fec7b26f3374eaa905e8696b45b9aabf31')

prepare() {
    cd ${srcdir}/${_dir}/..
    patch -p1 < ${srcdir}/fix-typesystem.patch
}

build() {
	# Use ROS environment variables.
	source /usr/share/ros-build-tools/clear-ros-env.sh
	[ -f /opt/ros/melodic/setup.bash ] && source /opt/ros/melodic/setup.bash

	# Create the build directory.
	[ -d ${srcdir}/build ] || mkdir ${srcdir}/build
	cd ${srcdir}/build

	# Fix Python2/Python3 conflicts.
	/usr/share/ros-build-tools/fix-python-scripts.sh -v 3 ${srcdir}/${_dir}

	# Build the project.
	cmake ${srcdir}/${_dir} \
		-DCMAKE_BUILD_TYPE=Release \
		-DCATKIN_BUILD_BINARY_PACKAGE=ON \
		-DCMAKE_INSTALL_PREFIX=/opt/ros/melodic \
		-DPYTHON_EXECUTABLE=/usr/bin/python3 \
		-DSETUPTOOLS_DEB_LAYOUT=OFF

	make
}

package() {
	cd "${srcdir}/build"
	make DESTDIR="${pkgdir}/" install
}
