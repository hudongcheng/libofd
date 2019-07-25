PROJECT(libofd)

CMAKE_MINIMUM_REQUIRED(VERSION 3.6)

# Install libzip tinyxml2 cairo  with brew
# brew install tinyxml2 cairo libzip

# For cairo depends on libffi, run below commands.
# export LDFLAGS="-L/usr/local/opt/libffi/lib"
# export PKG_CONFIG_PATH="/usr/local/opt/libffi/lib/pkgconfig"

find_package(PkgConfig)
pkg_search_module(LIBZIPPC REQUIRED libzip)
pkg_search_module(XMLPC REQUIRED tinyxml2)
pkg_search_module(CAIROPC REQUIRED cairo)

MESSAGE(STATUS ${LIBZIPPC_LIBRARIES})
MESSAGE(STATUS ${LIBZIPPC_INCLUDE_DIRS})
MESSAGE(STATUS ${XMLPC_INCLUDE_DIRS})
MESSAGE(STATUS ${CAIROPC_INCLUDE_DIRS})

INCLUDE_DIRECTORIES(
    ./include
    ${LIBZIPPC_INCLUDE_DIRS}
    ${XMLPC_INCLUDE_DIRS}
    /usr/local/Cellar/cairo/1.16.0/include/
    ${CAIROPC_INCLUDE_DIRS}
)

LINK_DIRECTORIES(
    /usr/local/lib
)

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
SET(LIBOFD_DEPENDICES freetype cairo tinyxml2 zip stdc++)

# cmake policy CMP0042
# ``MACOSX_RPATH`` is enabled by default
SET(CMAKE_MACOSX_RPATH 0)

SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
SET(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)

ADD_SUBDIRECTORY(src)
ADD_SUBDIRECTORY(tests)

ADD_TEST(ofdtest ${PROJECT_BINARY_DIR}/bin/ofdtest)
ENABLE_TESTING()

