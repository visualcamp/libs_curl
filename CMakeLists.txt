cmake_minimum_required(VERSION 3.5)
project(curl)

message("============================================================")
message(">>>                   Configuring Curl                   <<<")

add_library(curl INTERFACE)

set(CURL_PATH ${CMAKE_CURRENT_SOURCE_DIR})
if (ANDROID)
  set(CURL_LIB "${CURL_PATH}/lib/android/${CMAKE_ANDROID_ARCH_ABI}/libcurl.a")

elseif (EMSCRIPTEN)
  set(CURL_LIB "${CURL_PATH}/lib/emscripten/libcurl.a")

elseif(WIN32)
  set(CURL_LIB "${CURL_PATH}/lib/windows/libcurl.lib")

elseif(${CMAKE_SYSTEM_NAME} MATCHES "iOS")
  set(CURL_LIB "${CURL_PATH}/lib/ios/libcurl.a")

elseif (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
  message(WARNING "Building on MacOS: Using system installed libcurl")

  find_package(CURL REQUIRED)
  set(CURL_LIB "-lcurl")
  set(CURL_INCLUDE_PATH "")

elseif (UNIX)
  message(WARNING "Building on Unix: Using system installed libcurl")

  find_package(CURL REQUIRED)
  set(CURL_LIB "-lcurl")
  set(CURL_INCLUDE_PATH "")

else()
  message(FATAL_ERROR "Unknown platform")
endif()

set (CURL_INCLUDE_PATH "${CURL_PATH}/include")
message(" - Curl library path: ${CURL_LIB}")
message(" - Curl incude  path: ${CURL_INCLUDE_PATH}")
message(">>>                                                      <<<")
message("============================================================")

target_link_libraries(curl INTERFACE ${CURL_LIB})
target_include_directories(curl INTERFACE ${CURL_INCLUDE_PATH})
