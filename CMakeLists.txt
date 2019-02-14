# 设置最低版本号
cmake_minimum_required(VERSION 3.11 FATAL_ERROR)
# 设置项目名称
project(rk3588-demo VERSION 0.0.1 LANGUAGES CXX)

# 输出系统信息
message(STATUS "System: ${CMAKE_SYSTEM_NAME} ${CMAKE_SYSTEM_VERSION}")

# 设置编译器
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 设置库架构
set(LIB_ARCH "aarch64")
set(DEVICE_NAME "RK356X")

link_directories(
    mpp_libs
    libs
    /userdata/ZLMediaKit/release/linux/Debug/
    # /usr/local/lib
)

# 寻找OpenCV库，使用自定义的OpenCV_DIR
set(3RDPARTY_PATH ${CMAKE_CURRENT_SOURCE_DIR}/3rdparty)
set(RGA_DIR ${3RDPARTY_PATH}/rga/${DEVICE_NAME})
set(RGA_LIB ${RGA_DIR}/lib/Linux/${LIB_ARCH}/librga.so)

set(MPP_PATH /home/marc/mpp-develop/)
set(ZLMEDIA_PATH /userdata/ZLMediaKit/)

# 用来搜索头文件的目录
include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/include

    ${ZLMEDIA_PATH}/api/include
    ${ZLMEDIA_PATH}/3rdpart/media-server/librtmp/include
    ${ZLMEDIA_PATH}/3rdpart/media-server/libflv/include
    ${ZLMEDIA_PATH}/3rdpart/sdk/include
    ${MPP_PATH}/
    ${RGA_DIR}/include
    ${MPP_PATH}/inc
    ${MPP_PATH}/osal/inc
    ${MPP_PATH}/utils
    ${MPP_PATH}/mpp/inc
    ${MPP_PATH}/mpp/base/inc

    /usr/local/include/Network
    /usr/local/include/Util
    /usr/local/include/Thread

    /userdata/metaRTC/metaRTC/include
    /userdata/metaRTC/metaRTC/libmetartccore7/src
)

# 应用运行前检查当前用户是否有权限, 不然就要使用sudo

add_executable(vi_3568_rtmp src/main.cpp src/h264_to_rtmp.cpp src/Common/macros.cpp src/simple_rtmp_pusher.cpp)
target_link_libraries(vi_3568_rtmp pthread dl rga rockchip_mpp utils ZLToolKit rtmp aio flv avformat avutil avcodec)

# add_executable(vi_3568_rtmp src/main.cpp src/rtmp_streamer.cpp src/Common/macros.cpp)
# target_link_libraries(vi_3568_rtmp pthread dl rga rockchip_mpp utils ZLToolKit rtmp aio flv)

add_executable(rtmp_test src/rtmp_pusher.cpp src/Common/macros.cpp )
target_link_libraries(rtmp_test pthread dl rga rockchip_mpp utils ZLToolKit rtmp aio flv avformat avutil)

# add_executable(rtmp_streamer src/rtmp_streamer.cpp src/Common/macros.cpp)
# target_link_libraries(rtmp_streamer pthread dl rga rockchip_mpp utils ZLToolKit rtmp aio flv)

# add_executable(rtc_test src/rtc_streamer.cpp )
# target_link_libraries(rtc_test pthread dl rga rockchip_mpp utils ZLToolKit rtmp aio flv metartc7 metartccore7)
