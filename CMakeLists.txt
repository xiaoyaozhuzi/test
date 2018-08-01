CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

PROJECT("opencl_test")

SET(CMAKE_SYSTEM_NAME android)

SET(TARGET aarch64)
SET(CL_TARGET_OPENCL_VERSION 120)


IF(${TARGET} MATCHES "aarch64")
    SET(NDK_STANDALONE_TOOLCHAIN /Users/zhangshuo/debris/android/bin/android-r16-toolchain/)
    SET(CMAKE_C_COMPILER ${NDK_STANDALONE_TOOLCHAIN}/bin/aarch64-linux-android-gcc)
    SET(CMAKE_CXX_COMPILER ${NDK_STANDALONE_TOOLCHAIN}/bin/aarch64-linux-android-g++)
    SET(SYSROOT ${NDK_STANDALONE_TOOLCHAIN}/neuropilot-sysroot)
    SET(CMAKE_FIND_ROOT_PATH ${NDK_STANDALONE_TOOLCHAIN})
    SET(CMAKE_C_FLAGS "${LINUX_FLAGS}  -g -O0 --sysroot=${SYSROOT}")
    SET(CMAKE_CXX_FLAGS "${LINUX_FLAGS}  -g -O0 --sysroot=${SYSROOT} -isystem ${NDK_STANDALONE_TOOLCHAIN}/include/c++/4.9.x")
    INCLUDE_DIRECTORIES(${SYSROOT}/usr/include/tensorflow/contrib/lite/schema)
    INCLUDE_DIRECTORIES(${SYSROOT}/usr/include/)
    LINK_DIRECTORIES(${CMAKE_CURRENT_LIST_DIR}/libs/arm64)
ELSEIF(${TARGET} MATCHES "arm")
    SET(NDK_STANDALONE_TOOLCHAIN /Users/zhangshuo/debris/android/bin/android-r16-toolchain-arm32/)
    SET(CMAKE_C_COMPILER ${NDK_STANDALONE_TOOLCHAIN}/bin/arm-linux-androideabi-gcc)
    SET(CMAKE_CXX_COMPILER ${NDK_STANDALONE_TOOLCHAIN}/bin/arm-linux-androideabi-g++)
    SET(SYSROOT ${NDK_STANDALONE_TOOLCHAIN}/neuropilot-sysroot)
    SET(CMAKE_FIND_ROOT_PATH ${NDK_STANDALONE_TOOLCHAIN})
    SET(CMAKE_C_FLAGS "${LINUX_FLAGS} -D__ANDROID_LINUX__ -Wno-attributes --sysroot=${SYSROOT}")
    SET(CMAKE_CXX_FLAGS "${LINUX_FLAGS} -D__ANDROID_LINUX__ -Wno-attributes --sysroot=${SYSROOT}")
ELSEIF(${TARGET} MATCHES "zstest")
    SET(CMAKE_CXX_COMPILER /Users/zhangshuo/debris/android/bin/android-r16-toolchain//bin/aarch64-linux-android-g++)
ENDIF()

INCLUDE_DIRECTORIES(./DesignPattern)

SET(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -fPIE -pie")
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11 -Wl,--unresolved-symbols=ignore-all")

ADD_EXECUTABLE(optest   DesignPattern/builder.cpp)
TARGET_LINK_LIBRARIES(optest OpenCL)

ADD_EXECUTABLE(Dijkstra_test  Dijkstra/oclDijkstra.cpp Dijkstra/oclDijkstraKernel.cpp)
TARGET_LINK_LIBRARIES(Dijkstra_test OpenCL)

ADD_EXECUTABLE(infoopencl   OpenCLInfo/OpenCLInfo.cpp)
TARGET_LINK_LIBRARIES(infoopencl OpenCL)
