project(ObjRecRANSAC)
cmake_minimum_required(VERSION 2.6)

find_package(VTK)

IF(NOT VTK_FOUND)
  message(FATAL_ERROR "VTK not found. VTK is not installed or cmake can not find it. Install VTK first and then try again.
If VTK is installed but cmake can not find it, set the VTK_DIR entry to the folder which contains the file VTKConfig.cmake")
ENDIF(NOT VTK_FOUND)

SET(_ORR_CUR_     ModelDatabase.cpp ObjRecRANSAC.cpp)
FILE(GLOB _ORR_ALGS_    Algorithms/*.cpp)
FILE(GLOB _ORR_DS_      DataStructures/*.cpp)
FILE(GLOB _ORR_DS_HTBS_ DataStructures/HashTableBoxStruct/*.cpp)
FILE(GLOB _ORR_DS_HTKT_ DataStructures/HashTableKdTree/*.cpp)
FILE(GLOB _ORR_DS_OKD_  DataStructures/ORRKdTree/*.cpp)
FILE(GLOB _ORR_DS_ORRO_ DataStructures/ORROctree/*.cpp)
FILE(GLOB _ORR_DS_RI_   DataStructures/RangeImage/*.cpp)
FILE(GLOB _ORR_SH_      Shapes/*.cpp)

IF(USE_CUDA)
  LIST(APPEND _ORR_CUR_ AcceptHypothesesAlgo.cpp)
ENDIF(USE_CUDA)


include_directories(
	${VTK_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
	/usr/local/cuda/include
	..
)

if(CMAKE_COMPILER_IS_GNUCXX)
    set(CMAKE_CXX_FLAGS "-Wno-deprecated -mfpmath=sse -msse -msse2 -msse3 -mssse3 -msse4.1 -O3")
endif()

add_library(ObjRecRANSAC SHARED
	@_ORR_CUR_@
	@_ORR_ALGS_@
	@_ORR_DS_@
	@_ORR_DS_HTBS_@
	@_ORR_DS_HTKT_@
	@_ORR_DS_OKD_@
	@_ORR_DS_ORRO_@
	@_ORR_DS_RI_@
	@_ORR_SH_@
)
target_link_libraries(ObjRecRANSAC ${OpenCV_LIBS} BasicTools BasicToolsL1)

add_library(ObjRecRANSACStatic STATIC
	@_ORR_CUR_@
	@_ORR_ALGS_@
	@_ORR_DS_@
	@_ORR_DS_HTBS_@
	@_ORR_DS_HTKT_@
	@_ORR_DS_OKD_@
	@_ORR_DS_ORRO_@
	@_ORR_DS_RI_@
	@_ORR_SH_@
)
set_target_properties(ObjRecRANSACStatic PROPERTIES OUTPUT_NAME ObjRecRANSAC)

