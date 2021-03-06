cmake_minimum_required(VERSION 3.5)

project(vulkan-tutorial)

option(ASSET_PATH
        "Relative or absolute path to asset location, trailing slash."
        "${CMAKE_SOURCE_DIR}/assets/")

LIST(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/CMake/modules/")
include(cotire)

FILE(GLOB_RECURSE AssetFiles "assets/**/*")
add_custom_target(assets SOURCES ${AssetFiles})

find_package(Vulkan REQUIRED)
find_package(GLFW REQUIRED)
find_package(GLM REQUIRED)
find_package(Boost REQUIRED)

add_definitions(-std=c++14)

add_executable(vulkan-tutorial src/main.cpp)
target_sources(vulkan-tutorial PUBLIC
    src/HelloTriangleApplication.hpp
    src/QueueFamilyIndices.hpp
    src/SwapChainSupportDetails.hpp
    src/UniformBufferObject.hpp
    src/VDeleter.hpp
    src/Vertex.hpp
    src/vkFunctions.hpp
    src/VMappedMemory.hpp
    src/settings.hpp
    src/pbbastian/stbi.hpp
    src/pbbastian/vulkan/UniqueObject.hpp
    src/pbbastian/vulkan/destroy.hpp
    src/pbbastian/vulkan/util.hpp
    src/pbbastian/vulkan/functions.hpp
    src/pbbastian/vulkan/ObjectOwner.hpp)
#target_compile_features(vulkan-tutorial )
target_include_directories(vulkan-tutorial
    PUBLIC
    src/
    vendor/stb/
    vendor/Vulkan-Hpp/
    vendor/tinyobjloader/
    ${VULKAN_INCLUDE_DIR}
    ${GLFW_INCLUDE_DIR}
    ${GLM_INCLUDE_DIR}
    ${BOOST_INCLUDE_DIRS})
target_link_libraries(vulkan-tutorial PUBLIC ${VULKAN_LIBRARY} ${GLFW_LIBRARIES} ${BOOST_LIBRARIES})
target_compile_definitions(vulkan-tutorial
        PUBLIC
        GLFW_INCLUDE_VULKAN
        GLM_FORCE_RADIANS
        GLM_FORCE_DEPTH_ZERO_TO_ONE
        STB_IMAGE_IMPLEMENTATION
        TINYOBJLOADER_IMPLEMENTATION
        VULKAN_HPP_NO_EXCEPTIONS
        ASSET_PATH="${CMAKE_SOURCE_DIR}/assets/")
#cotire(vulkan-tutorial)
