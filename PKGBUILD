# Maintainer: Timon Engelke <aur@timonengelke.de>
pkgdesc="ROS - Python interfaces to MoveIt."
url='https://moveit.ros.org'

pkgname='ros-melodic-moveit-commander'
pkgver='1.0.6'
arch=('any')
pkgrel=1
license=('BSD')

ros_makedepends=(ros-melodic-catkin
  ros-melodic-moveit-resources)
makedepends=('cmake' 'ros-build-tools'
  ${ros_makedepends[@]}
  python
  python-catkin-pkg)

ros_depends=(ros-melodic-moveit-msgs
  ros-melodic-tf
  ros-melodic-geometry-msgs
  ros-melodic-rospy
  ros-melodic-moveit-ros-planning-interface
  ros-melodic-sensor-msgs
  ros-melodic-shape-msgs)
depends=(${ros_depends[@]}
  python
  python-pyassimp)

# Tarball version (faster download)
_dir="moveit-${pkgver}/moveit_commander"
source=("${pkgname}-${pkgver}.tar.gz"::"https://github.com/ros-planning/moveit/archive/${pkgver}.tar.gz")
sha256sums=('a633830d2ed7e23089f9642d99298cb6eb96148c695c0b4890f2792eac4904b4')

build() {
  # Use ROS environment variables
  source /usr/share/ros-build-tools/clear-ros-env.sh
  [ -f /opt/ros/melodic/setup.bash ] && source /opt/ros/melodic/setup.bash

  # Create build directory
  [ -d ${srcdir}/build ] || mkdir ${srcdir}/build
  cd ${srcdir}/build

  # Fix Python2/Python3 conflicts
  /usr/share/ros-build-tools/fix-python-scripts.sh -v 3 ${srcdir}/${_dir}

  # Build project
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
