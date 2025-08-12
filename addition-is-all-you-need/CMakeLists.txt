cmake_minimum_required(VERSION 3.24)
project(tile_and_error_kernels LANGUAGES C CXX CUDA)

set(SRC_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src)
set(TEST_DIR ${CMAKE_CURRENT_SOURCE_DIR}/tests)

add_cuda_app(bench
  ${SRC_DIR}/main.cpp
  ${SRC_DIR}/kernel.cu
  ${SRC_DIR}/ref_impl.cpp
)

add_executable(tests
  ${TEST_DIR}/correctness.cpp
  ${SRC_DIR}/ref_impl.cpp
  ${SRC_DIR}/kernel.cu
)
target_link_libraries(tests PRIVATE thunderkittens::tk)
add_test(NAME quick_tests COMMAND tests)
