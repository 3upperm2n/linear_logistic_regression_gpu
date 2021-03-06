### CMakeLists.txt for CUDA
### https://gforge.sci.utah.edu/gf/project/findcuda/scmsvn/?action=browse&path=%2Ftrunk%2FCMake%2Fcuda%2FFindCUDA.cmake&view=markup
cmake_minimum_required(VERSION 2.8)

project("Linear Regression on CUDA") 

# find cuda installed
find_package(CUDA QUIET REQUIRED)
if (CUDA_FOUND)
	message("CUDA found, using device squaring!")
else()
	message("CUDA not found, doing something alternatively")
endif()

### find cublas
find_cuda_helper_libs(cublas)
set(CUDA_CUBLAS_LIBRARIES ${CUDA_cublas_LIBRARY})

# versose during building
set(CUDA_VERBOSE_BUILD on)

# nvcc options 
set(CUDA_NVCC_FLAGS ${CUDA_NVCC_FLAGS};
-O2
-gencode arch=compute_30,code=sm_30 
-gencode arch=compute_35,code=sm_35 
-gencode arch=compute_50,code=sm_50
-gencode arch=compute_52,code=sm_52
)

# include header files 
include_directories(
/usr/local/cuda/include
/usr/local/cuda/samples/common/inc
${CMAKE_CURRENT_SOURCE_DIR}
)

# libs 
link_directories(
/usr/local/cuda/lib64	
)

# compile target from source files 
cuda_add_executable(
logistic_reg_cuda
lgr.cu
)

# runtime libs
target_link_libraries(
logistic_reg_cuda
-lcudart
${CUDA_cublas_LIBRARY}
)
