cmake_minimum_required(VERSION 3.1.0)
project(BOSS_MQTT_bridge VERSION 0.1.0)

include(CTest)
enable_testing()

################################# Dependencies #################################
include(ExternalProject)

set(BOSS_GITHUB_REPOSITORY_USERNAME "" CACHE STRING "BOSS_GITHUB_REPOSITORY_USERNAME")
set(BOSS_GITHUB_REPOSITORY_PASSWORD "" CACHE STRING "BOSS_GITHUB_REPOSITORY_PASSWORD")

ExternalProject_Add(boss
	URL "https://github.com/symbol-store/BOSS/archive/master.zip"
  HTTP_USERNAME ${BOSS_GITHUB_REPOSITORY_USERNAME}
  HTTP_PASSWORD ${BOSS_GITHUB_REPOSITORY_PASSWORD}
  DOWNLOAD_DIR $ENV{HOME}/.cmake-downloads
	CMAKE_ARGS -DCMAKE_INSTALL_PREFIX=${BOSS_MQTT_bridge_BINARY_DIR}/deps -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE} -DCMAKE_CXX_COMPILER=${CMAKE_CXX_COMPILER}  -DCMAKE_C_COMPILER=${CMAKE_C_COMPILER} -DDCMAKE_CXX_FLAGS=${DCMAKE_CXX_FLAGS} -DBENCHMARK_USE_LIBCXX=${GOOGLEBENCHMARK_USE_LIBCXX}
  )

################################## Executables #################################

add_executable(BOSS_MQTT_bridge main.cpp)
target_include_directories(BOSS_MQTT_bridge SYSTEM PUBLIC ${BOSS_MQTT_bridge_BINARY_DIR}/deps/include)
set_target_properties(BOSS_MQTT_bridge PROPERTIES CXX_STANDARD 17 CXX_STANDARD_REQUIRED ON CXX_EXTENSIONS OFF)
add_dependencies(BOSS_MQTT_bridge boss)

# K: 
# https://stackoverflow.com/questions/55799122/compile-pahomqttcpp-sample-as-standalone-with-cmake-on-linux/55800526#55800526
# https://stackoverflow.com/questions/40668959/how-to-link-to-mosquitto-lib-in-cmake
find_package(PkgConfig REQUIRED)
pkg_check_modules(Mosquitto REQUIRED IMPORTED_TARGET libmosquitto)
target_link_libraries(BOSS_MQTT_bridge PkgConfig::Mosquitto)


#add_executable(BOSS_MQTT_bridge main.cpp)

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)
