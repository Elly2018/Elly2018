cmake_minimum_required(VERSION 3.10)

set(Root "${CMAKE_CURRENT_SOURCE_DIR}")
set(Main "${Root}/main")
set(BIN_DIR "${Root}/bin")
set(Lib "${Root}/third_party")
set(Platform "${Root}/platform")

## Project naming
project(Ellyality VERSION 0.0.1 LANGUAGES CXX)

# App Executable
file(GLOB_RECURSE App_Files 
	"${Main}/*.h"
	"${Main}/*.hpp"
	"${Main}/*.c"
	"${Main}/*.cpp")
add_executable(Ellyality ${App_Files})

set_target_properties(Ellyality PROPERTIES ARCHIVE_OUTPUT_DIRECTORY_DEBUG "${BIN_DIR}")
set_target_properties(Ellyality PROPERTIES LIBRARY_OUTPUT_DIRECTORY_DEBUG "${BIN_DIR}")
set_target_properties(Ellyality PROPERTIES RUNTIME_OUTPUT_DIRECTORY_DEBUG "${BIN_DIR}")
set_target_properties(Ellyality PROPERTIES ARCHIVE_OUTPUT_DIRECTORY_RELEASE "${BIN_DIR}")
set_target_properties(Ellyality PROPERTIES LIBRARY_OUTPUT_DIRECTORY_RELEASE "${BIN_DIR}")
set_target_properties(Ellyality PROPERTIES RUNTIME_OUTPUT_DIRECTORY_RELEASE "${BIN_DIR}")

## Platform detection
if (WIN32)
    target_include_directories(Ellyality PUBLIC "${Platform}/windows")
elseif(LINUX)
    target_include_directories(Ellyality PUBLIC "${Platform}/linux")
elseif(APPLE)
    target_include_directories(Ellyality PUBLIC "${Platform}/apple")
endif()

target_include_directories(Ellyality PUBLIC "${Main}")
target_include_directories(Ellyality PUBLIC "$ENV{VK_SDK_PATH}/include")
target_include_directories(Ellyality PUBLIC "${Lib}/cglm/include")
target_include_directories(Ellyality PUBLIC "${Lib}/glfw/include")

target_link_directories(Ellyality PUBLIC "$ENV{VK_SDK_PATH}/Lib")
target_link_directories(Ellyality PUBLIC "${Lib}/cglm/Debug")
target_link_directories(Ellyality PUBLIC "${Lib}/glfw/src/Debug")

target_link_libraries(Ellyality PUBLIC "VkLayer_utils.lib")
target_link_libraries(Ellyality PUBLIC "vulkan-1.lib")
target_link_libraries(Ellyality PUBLIC "cglm.lib")
target_link_libraries(Ellyality PUBLIC "glfw3.lib")