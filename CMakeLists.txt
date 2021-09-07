cmake_minimum_required(VERSION 3.10)

# set project name and version
project(grpc_test_srv)

set (AR_VERSION_MAJOR 1)
set (AR_VERSION_MINOR 0)

#################################
# GDB 设置
#################################
set(CMAKE_VERBOSE_MAKEFILE on)
set(CMAKE_BUILD_TYPE "Debug")
set(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g -ggdb")
set(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall")

#################################
# 共享库 设置
#################################
# aux_source_directory(src SRC_LIST)
# add_subdirectory(${PROJECT_SOURCE_DIR}/src/lib)

set(SRC_LIST ${PROJECT_SOURCE_DIR}/src/helloworld.grpc.pb.cc
                ${PROJECT_SOURCE_DIR}/src/helloworld.pb.cc 
                ${PROJECT_SOURCE_DIR}/src/main.cpp
             )

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}  -std=c++0x")

if(APPLE)
    include_directories(${PROJECT_SOURCE_DIR}/src
                    /usr/local/include)
    link_directories(/usr/local/lib
                     /opt/homebrew/Cellar/openssl@1.1/1.1.1k/lib)
elseif(UNIX)
        include_directories(${PROJECT_SOURCE_DIR}/src/include
                    ${PROJECT_SOURCE_DIR}/src/include/http
                    ${PROJECT_SOURCE_DIR}/src/include/task
                    ${PROJECT_SOURCE_DIR}/src/include/rpc
                    ${PROJECT_SOURCE_DIR}/3rd_party/linux/include
                    /usr/local/include
                    /usr/local/include/opencv4)
        link_directories(/usr/local/lib
                     /usr/local/lib64
                     ${PROJECT_SOURCE_DIR}/3rd_party/linux/lib
                     /opt/homebrew/Cellar/openssl@1.1/1.1.1k/lib)
endif()

#################################
# 生成依赖库
#################################
# add_library()

#################################
# 生成可执行文件
#################################
add_executable(grpc_test_srv ${SRC_LIST})

# 添加链接库
target_link_libraries(grpc_test_srv
                      protobuf
                      grpc++
                      grpc++_reflection)