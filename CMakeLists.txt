cmake_minimum_required(VERSION 3.0.0)
project(BOSS_MQTT_bridge VERSION 0.1.0)

include(CTest)
enable_testing()

add_executable(BOSS_MQTT_bridge main.cpp)

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
