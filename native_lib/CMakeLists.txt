cmake_minimum_required(VERSION 3.16)

project(FreeyaAuerinCatherine_app_library VERSION 0.0.1 LANGUAGES C CXX)
set(CMAKE_CXX_STANDARD 20)
 
message(STATUS "START COMPILE PACKAGE_TEMPLATE")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC")

set(FreeyaAuerinCatherine_app_shared_dir "lib")
set(FreeyaAuerinCatherine_app_exe_dir "bin")
set(prefix_shared_library "lib")
 

if (ANDROID)
 set(package_shared_extension "_${ANDROID_ARCH_NAME}.so")
 set(package_exe_extension "")
 set(package_name "FreeyaAuerinCatherine_app_${ANDROID_ARCH_NAME}")
elseif (WIN32)
 set(package_shared_extension ".dll")
 set(package_exe_extension ".exe") 
 set(package_name "FreeyaAuerinCatherine_app")
elseif (APPLE)
 set(package_shared_extension ".dylib")
 set(package_exe_extension "") 
 set(package_name "FreeyaAuerinCatherine_app")
else()
 set(package_shared_extension ".so")
 set(package_exe_extension "") 
 set(package_name "FreeyaAuerinCatherine_app")
endif()


add_library(FreeyaAuerinCatherine_app_shared SHARED FreeyaAuerinCatherine_app.cpp)
add_executable(FreeyaAuerinCatherine_app_exe FreeyaAuerinCatherine_app.cpp)

set_target_properties(FreeyaAuerinCatherine_app_shared PROPERTIES 
  LIBRARY_OUTPUT_DIRECTORY "${FreeyaAuerinCatherine_app_shared_dir}"
  PREFIX "${prefix_shared_library}"
  OUTPUT_NAME "${package_name}"
)

set_target_properties(FreeyaAuerinCatherine_app_exe PROPERTIES 
  RUNTIME_OUTPUT_DIRECTORY "${FreeyaAuerinCatherine_app_exe_dir}"
  OUTPUT_NAME "${package_name}"
)

if (WIN32)
 set_target_properties(FreeyaAuerinCatherine_app_shared PROPERTIES
  WINDOWS_EXPORT_ALL_SYMBOLS ON
)
endif (WIN32)
 
add_custom_command(
  TARGET FreeyaAuerinCatherine_app_shared POST_BUILD 
  COMMAND ${CMAKE_STRIP} --strip-debug --strip-unneeded "${FreeyaAuerinCatherine_app_shared_dir}/${prefix_shared_library}FreeyaAuerinCatherine_app${package_shared_extension}"
)

add_custom_command(
  TARGET FreeyaAuerinCatherine_app_exe POST_BUILD 
  COMMAND ${CMAKE_STRIP} --strip-debug --strip-unneeded "${FreeyaAuerinCatherine_app_exe_dir}/FreeyaAuerinCatherine_app${package_exe_extension}" 
)

 