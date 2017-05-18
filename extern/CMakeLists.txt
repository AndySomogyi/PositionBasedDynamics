add_subdirectory(glfw)


# Build the imgui library

# imgui src
#OBJS = main.o imgui_impl_glfw_gl3.o
#OBJS += ../../imgui.o ../../imgui_demo.o ../../imgui_draw.o
#OBJS += ../libs/gl3w/GL/gl3w.o

# opengl3 exmple:

# main.cpp
# 

set(imgui_DIR ${CMAKE_CURRENT_SOURCE_DIR}/imgui)
set(imgui_EXAMPLE_DIR ${imgui_DIR}/examples/opengl3_example)
set(GLFW_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/glfw)

set(imgui_SOURCES
  ${imgui_DIR}/imgui_draw.cpp
  ${imgui_DIR}/imgui.cpp
  ${imgui_DIR}/imgui_demo.cpp
  ${imgui_EXAMPLE_DIR}/imgui_impl_glfw_gl3.cpp
  )

set(imgui_HEADERS
  ${imgui_DIR}/imconfig.h
  ${imgui_DIR}/imgui.h
  ${imgui_DIR}/stb_rect_pack.h
  ${imgui_DIR}/stb_truetype.h
  ${imgui_DIR}/imgui_internal.h
  ${imgui_DIR}/stb_textedit.h
  ${imgui_EXAMPLE_DIR}/imgui_impl_glfw_gl3.h
  )


add_library(imgui STATIC ${imgui_SOURCES} ${imgui_HEADERS})
  
#set_target_properties(glfw PROPERTIES
#                      OUTPUT_NAME ${GLFW_LIB_NAME}
#                      VERSION ${GLFW_VERSION}
#                      SOVERSION ${GLFW_VERSION_MAJOR}
#                      POSITION_INDEPENDENT_CODE ON
#                      FOLDER "GLFW3")

#target_compile_definitions(glfw PRIVATE -D_GLFW_USE_CONFIG_H)

target_include_directories(imgui PUBLIC
  $<BUILD_INTERFACE:${imgui_DIR}>
  $<BUILD_INTERFACE:${imgui_EXAMPLE_DIR}>
  $<BUILD_INTERFACE:${GLFW_SOURCE_DIR}/deps>)

message("dir: ${GLFW_SOURCE_DIR}/deps")

target_link_libraries(imgui glfw)

# no files to install for imgui
# $<INSTALL_INTERFACE:$<INSTALL_PREFIX>/include>)

#target_include_directories(imgui PRIVATE
#                           "${imgui_DIR}"
#                           "${GLFW_BINARY_DIR}/src"
#                           ${glfw_INCLUDE_DIRS})


#if (WIN32)
#    set(ICON glfw.rc)
#elseif (APPLE)
#    set(ICON glfw.icns)
#    set_source_files_properties(glfw.icns PROPERTIES
#        MACOSX_PACKAGE_LOCATION "Resources")
#endif()

set(CMAKE_DEBUG_TARGET_PROPERTIES
  INCLUDE_DIRECTORIES
  COMPILE_DEFINITIONS
  POSITION_INDEPENDENT_CODE
  CONTAINER_SIZE_REQUIRED
  LIB_VERSION
)

set(GLAD "${GLFW_SOURCE_DIR}/deps/glad/glad.h"
         "${GLFW_SOURCE_DIR}/deps/glad.c")


add_executable(imgui_example WIN32 MACOSX_BUNDLE
  ${imgui_EXAMPLE_DIR}/main.cpp ${ICON} ${GLAD})

if (MSVC)
    target_add_definitions(imgui_example -D_CRT_SECURE_NO_WARNINGS)
endif()

if (GLFW_USE_OSMESA)
    target_add_definitions(imgui_example -DUSE_NATIVE_OSMESA)
endif()


target_link_libraries(imgui_example imgui)

set(WINDOWS_BINARIES imgui_example)

#set_target_properties(${WINDOWS_BINARIES} ${CONSOLE_BINARIES} PROPERTIES
#                      FOLDER "GLFW3/Examples")

if (MSVC)
    # Tell MSVC to use main instead of WinMain for Windows subsystem executables
    set_target_properties(${WINDOWS_BINARIES} PROPERTIES
                          LINK_FLAGS "/ENTRY:mainCRTStartup")
endif()

if (APPLE)
  set_target_properties(imgui_example
    PROPERTIES MACOSX_BUNDLE_BUNDLE_NAME "Imgui Example")
  
  set_target_properties(${WINDOWS_BINARIES} PROPERTIES
    RESOURCE glfw.icns
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${GLFW_VERSION}
    MACOSX_BUNDLE_LONG_VERSION_STRING ${GLFW_VERSION_FULL}
    MACOSX_BUNDLE_ICON_FILE glfw.icns
    MACOSX_BUNDLE_INFO_PLIST "${GLFW_SOURCE_DIR}/CMake/MacOSXBundleInfo.plist.in")
endif()



