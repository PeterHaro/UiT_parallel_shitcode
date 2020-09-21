cmake_minimum_required(VERSION 3.17)
project(cmake_test_openmp C)

set(CMAKE_C_STANDARD 99)
find_package(OpenMP)
if(OpenMP_CXX_FOUND)
    target_link_libraries(parallel_basic_nbody PUBLIC OpenMP::OpenMP_CXX)
endif()

find_package(OpenMP)
if (OPENMP_FOUND)
    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    set (CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif()

set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads REQUIRED)

add_executable(cmake_test_openmp main.c)
add_executable(basic_nbody nbody.c timer.h)
add_executable(parallel_basic_nbody mp_nbody.c timer.h)
add_executable(pthread_parallel_nbody pthread_nbody.c timer.h)

# TSP STUFF
add_executable(tsp_recursive recursive_tsp.c timer.h)
add_executable(tsp_iterative iterative_tsp.c timer.h)

add_executable(tsp_parallel_pthread pthread_tsp.c)
add_executable(tsp_parallel_omp omp_tsp.c)

target_link_libraries(pthread_parallel_nbody PRIVATE Threads::Threads)
target_link_libraries(tsp_parallel_pthread PRIVATE Threads::Threads)