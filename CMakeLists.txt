project(logger2)

cmake_minimum_required(VERSION 2.8)
set(CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake;${CMAKE_MODULE_PATH}")

find_package(ZLIB REQUIRED)
find_package(Qt5Widgets REQUIRED)
find_package(OpenCV REQUIRED)
find_package(Boost COMPONENTS thread REQUIRED)
find_package(Boost COMPONENTS filesystem REQUIRED)
find_package(Boost COMPONENTS system REQUIRED)
find_package(Boost COMPONENTS date_time REQUIRED)
find_package(OpenNI2 REQUIRED)

qt5_wrap_cpp(main_moc_SRCS src/main.h)

INCLUDE_DIRECTORIES(${Boost_INCLUDE_DIR} ${OPENNI2_INCLUDE_DIR} ${Qt5Widgets_INCLUDE_DIRS})
INCLUDE_DIRECTORIES(${ZLIB_INCLUDE_DIR})

set(Boost_USE_STATIC_LIBS OFF)
set(Boost_USE_MULTITHREADED ON)
set(Boost_USE_STATIC_RUNTIME OFF)
set(BOOST_ALL_DYN_LINK ON)   # force dynamic linking for all libraries

set(CMAKE_CXX_FLAGS "-O3 -msse2 -msse3 -std=c++11 -fPIC")

set(CMAKE_EXE_LINKER_FLAGS "-Wl,-rpath,. -L${CMAKE_CURRENT_BINARY_DIR}")

set(srcs
  src/main.cpp
  src/Logger2.cpp
  src/Communicator.cpp
  src/OpenNI2Interface.cpp)

add_executable(Logger2 ${main_moc_SRCS} ${srcs})

target_link_libraries(Logger2
                      ${ZLIB_LIBRARY}
                      ${Boost_LIBRARIES}
                      ${OpenCV_LIBS}
                      ${QT_LIBRARIES}
                      boost_system
                      boost_filesystem
                      boost_thread
                      ${OPENNI2_LIBRARY}
                      ${Qt5Widgets_LIBRARIES}
                      )
