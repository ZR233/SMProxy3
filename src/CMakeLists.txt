# CMakeList.txt : 顶级 CMake 项目文件，执行全局配置
# 和此处的包含子项目。
#
cmake_minimum_required (VERSION 3.8)


aux_source_directory(. DIR_SRC_SRCS)
add_library(SMProxy SHARED ${DIR_SRC_SRCS})








if(WINDOWS)
    # Copy DLL files to output directory
    if(CMAKE_CL_64)
        add_custom_command(TARGET SMProxy POST_BUILD 
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
        "${PROJECT_SOURCE_DIR}/platforms/win/SMProxyx64.dll"              
        $<TARGET_FILE_DIR:SMProxy>)
    else()
        add_custom_command(TARGET SMProxy POST_BUILD 
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
        "${PROJECT_SOURCE_DIR}/platforms/win/SMProxyx86.dll"              
        $<TARGET_FILE_DIR:SMProxy>)
    endif()
endif()


install (TARGETS SMProxy DESTINATION bin)