
# Centos 7
* python3
https://www.digitalocean.com/community/tutorials/how-to-install-python-3-and-set-up-a-local-programming-environment-on-centos-7
* protobufs https://blog.jeffli.me/blog/2016/12/08/install-protocol-buffer-from-source-in-centos-7/

* Install gazebo dependencies
http://www.gazebosim.org/tutorials?tut=install_dependencies_from_source&cat=install#InstalldependenciesfromsourceonUbuntu

install cmake3 
yum install cmake3

# Ignition Math
For ign-math you will get an error doing The compiler feature "cxx_attribute_deprecated" is
not known to CXX
if using gcc 4.8.5, upgrade to gcc 5.4.0
https://jdhao.github.io/2017/09/04/install-gcc-newer-version-on-centos/

cmake3 -DCMAKE_C_COMPILER=/usr/local/bin/gcc
-DCMAKE_CXX_COMPILER=/usr/local/bin/g++ ..t /

# ignition common
yum install freeimage, tinyxml2, uuid-devel
gts?
libavdevice
libavformat?

ffmpeg-devel has libavcodec?

From source ign-math5 was built had to change CMakeList from v4 to v5

GTS has to be installed from source
http://gts.sourceforge.net/install.html

For the package to be found must export the path
export PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig


