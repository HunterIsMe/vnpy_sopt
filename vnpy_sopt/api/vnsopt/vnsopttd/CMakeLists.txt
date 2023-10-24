#使用方法(符号">>>"后面的表示执行的命令)
#定义python路径 >>> $Env:PYTHON_PATH = "******"
#定义python链接库的名称 >> $Env:PYTHON_LIB = "****"
#进入build目录,构建(生成)项目 >>> cmake ..
#编译(debug模式) >>> cmake --build . --config Debug
#编译(release模式) >>> cmake --build . --config Release

#备注:如果python版本和pybind11的版本相差太远，会编译失败
#PYTHON_PATH是执行环境使用的那个python解释器所在目录
#PYTHON_LIB是{PYTHON_PATH}//libs里的那个.lib后缀文件，比如python311.lib

cmake_minimum_required(VERSION 3.10)  # 指定CMake的最低版本

message(STATUS "cmake start ................")
message(STATUS "python path: $ENV{PYTHON_PATH}")
message(STATUS "python lib name: $ENV{PYTHON_LIB}")
message(STATUS "CMAKE_BUILD_TYPE: ${CMAKE_BUILD_TYPE}")

set(PROJ_NAME "vnsopttd")
message(STATUS "PROJ_NAME: ${PROJ_NAME}")

project(${PROJ_NAME})  # 设置项目名称

# 设置生成的库为共享库（DLL）
add_library(${PROJ_NAME} SHARED ${PROJ_NAME}.cpp)

# 可选：设置输出文件的名称和位置
set_target_properties(${PROJ_NAME} PROPERTIES
    OUTPUT_NAME ${PROJ_NAME}
    LIBRARY_OUTPUT_DIRECTORY_RELEASE ${CMAKE_SOURCE_DIR}/../..
    RUNTIME_OUTPUT_DIRECTORY_RELEASE ${CMAKE_SOURCE_DIR}/../..
    LIBRARY_OUTPUT_DIRECTORY_DEBUG ${CMAKE_SOURCE_DIR}/../..
    RUNTIME_OUTPUT_DIRECTORY_DEBUG ${CMAKE_SOURCE_DIR}/../..
    SUFFIX ".pyd"
)


# 可选：定义一些预处理变量，以便在代码中进行条件编译
target_compile_definitions(${PROJ_NAME} PRIVATE NOMINMAX _CRT_SECURE_NO_WARNINGS)

#依赖库所在目录
target_link_directories(${PROJ_NAME} PRIVATE  $ENV{PYTHON_PATH}/libs)
target_link_directories(${PROJ_NAME} PRIVATE  ${CMAKE_SOURCE_DIR}/../../libs)

#添加依赖库
target_link_libraries(${PROJ_NAME} PRIVATE python3.lib)
target_link_libraries(${PROJ_NAME} PRIVATE $ENV{PYTHON_LIB})
target_link_libraries(${PROJ_NAME} PRIVATE soptthosttraderapi_se.lib)

# 指定头文件的位置
target_include_directories(${PROJ_NAME} PRIVATE $ENV{PYTHON_PATH}/include)
target_include_directories(${PROJ_NAME} PRIVATE ${CMAKE_SOURCE_DIR})
target_include_directories(${PROJ_NAME} PRIVATE ${CMAKE_SOURCE_DIR}/..)
target_include_directories(${PROJ_NAME} PRIVATE ${CMAKE_SOURCE_DIR}/../../include)
