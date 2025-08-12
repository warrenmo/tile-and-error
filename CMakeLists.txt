cmake_minimum_required(VERSION 3.24)
project(tile_and_error_super LANGUAGES C CXX CUDA)

set(CMAKE_CUDA_STANDARD 17)
set(CMAKE_CXX_STANDARD 17)

set(CUDA_ARCH "sm_80" CACHE STRING "CUDA arch, e.g., sm_75, sm_80, sm_89, sm_90a")

find_program(CCACHE_PROGRAM ccache)
if (CCACHE_PROGRAM)
  set(CMAKE_C_COMPILER_LAUNCHER   "${CCACHE_PROGRAM}")
  set(CMAKE_CXX_COMPILER_LAUNCHER "${CCACHE_PROGRAM}")
  set(CMAKE_CUDA_COMPILER_LAUNCHER "${CCACHE_PROGRAM}")
endif()

set(TK_ROOT "${CMAKE_SOURCE_DIR}/ThunderKittens")
if (NOT EXISTS "${TK_ROOT}/include")
  message(FATAL_ERROR "ThunderKittens not found at ${TK_ROOT}. Did you init submodules?")
endif()

add_library(thunderkittens INTERFACE)
target_include_directories(thunderkittens INTERFACE "${TK_ROOT}/include")
add_library(thunderkittens::tk ALIAS thunderkittens)

function(add_cuda_app target)
  add_executable(${target} ${ARGN})
  target_compile_options(${target} PRIVATE
    $<$<COMPILE_LANGUAGE:CUDA>:
      -O3
      --use_fast_math
      --ptxas-options=-v
      --generate-line-info
      -arch=${CUDA_ARCH}
    >
  )
  target_link_libraries(${target} PRIVATE thunderkittens::tk)
endfunction()

enable_testing()

add_subdirectory(addition-is-all-you-need)
