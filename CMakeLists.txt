# CMakeList.txt : 顶级 CMake 项目文件，执行全局配置
# 和此处的包含子项目。
#
cmake_minimum_required (VERSION 3.8)

project (SMProxy)
MESSAGE( STATUS "PROJECT_NAME: " ${PROJECT_NAME} )


add_subdirectory(src)


# The version number.
set(SMProxy_VERSION_MAJOR 1)
set(SMProxy_VERSION_MINOR 0)




# Check platforms
if (CMAKE_HOST_WIN32)
    set(WINDOWS 1)
elseif(CMAKE_HOST_APPLE)
    set(MACOS 1)
elseif(CMAKE_HOST_UNIX)
    set(LINUX 1)
endif()

# Set RPATH
if(WINDOWS)
else()
    set(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib")
    MESSAGE( STATUS "CMAKE_INSTALL_RPATH: " "${CMAKE_INSTALL_PREFIX}/lib" )
    set(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)
endif()

# Configure a header file to pass some of the CMake settings
# to the source code
configure_file (
    "${PROJECT_SOURCE_DIR}/SMProxyConfig.h.in"
    "${PROJECT_BINARY_DIR}/SMProxyConfig.h"
)

# Add search path for include and lib files
if(WINDOWS)
    link_directories("${PROJECT_SOURCE_DIR}/platforms/win") 
elseif(LINUX)
    link_directories("${PROJECT_SOURCE_DIR}/platforms/linux") 
elseif(MACOS)
    link_directories("${PROJECT_SOURCE_DIR}/platforms/macos") 
endif()
include_directories("${PROJECT_BINARY_DIR}" "${PROJECT_SOURCE_DIR}/include")



#if(WINDOWS)
#	if(CMAKE_CL_64)
#        target_link_libraries (SMProxy "DBRx64")
#	else()
#		target_link_libraries (SMProxy "DBRx86")
#    endif()
#else()
#    target_link_libraries (SMProxy "SMProxy")
#endif()



# Set installation directory
if(WINDOWS)
    set(CMAKE_INSTALL_PREFIX "d:/${PROJECT_NAME}")
    if(CMAKE_CL_64)
    install (FILES "${PROJECT_SOURCE_DIR}/platforms/win/SMProxyx64.dll" DESTINATION bin)
    else()
    install (FILES "${PROJECT_SOURCE_DIR}/platforms/win/SMProxyx86.dll" DESTINATION bin)
    endif()
elseif(LINUX)
    install (FILES "${PROJECT_SOURCE_DIR}/platforms/linux/libSMProxy.so" DESTINATION lib)
elseif(MACOS)
    install (FILES "${PROJECT_SOURCE_DIR}/platforms/macos/libSMProxy.dylib" DESTINATION lib)
endif()


install (FILES "${PROJECT_BINARY_DIR}/SMProxyConfig.h" DESTINATION include)
install (DIRECTORY "${PROJECT_SOURCE_DIR}/include" DESTINATION include)