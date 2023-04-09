cmake_minimum_required(VERSION 3.0)

set(CMAKE_INSTALL_PREFIX "${CMAKE_CURRENT_BINARY_DIR}/install" CACHE PATH "Path where to install binaries")

project(caffe-windows-dependencies)

include (ExternalProject)

find_package(Boost 1.60.0 REQUIRED COMPONENTS system thread filesystem date_time)

#if(MINGW)
#  add_definitions(-D__MINGW32__)
#endif()

# gflags
set(ExternalProject_GIT_REPOSITORY "https://github.com/gflags/gflags.git")
set(ExternalProject_GIT_TAG "master")
set (GFLAGS_CMAKE_ARGS 
     -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}/gflags # set the install prefix per generator
     -DCMAKE_DEBUG_POSTFIX=d # add a debug postfix to the debug libs so we can install them with the release ones
     -DBUILD_SHARED_LIBS=OFF # build static libs
	 UPDATE_DISCONNECTED 1
     )

ExternalProject_Add ( gflags 
						GIT_REPOSITORY ${ExternalProject_GIT_REPOSITORY}
						GIT_TAG ${ExternalProject_GIT_TAG}
						SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/gflags
						CMAKE_ARGS ${GFLAGS_CMAKE_ARGS}
                    )

set(GFLAGS_DIR ${CMAKE_INSTALL_PREFIX}/gflags/CMake)

# glog
set(ExternalProject_GIT_REPOSITORY "https://github.com/google/glog.git")
set(ExternalProject_GIT_TAG "master")
set (GLOG_CMAKE_ARGS 
     -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}/glog # set the install prefix per generator     
     -DBUILD_SHARED_LIBS=OFF # build static libs  
     -DCMAKE_DEBUG_POSTFIX=d # add a debug postfix to the debug libs so we can install them with the release ones
     -Dgflags_DIR=${CMAKE_INSTALL_PREFIX}/gflags/CMake
	 UPDATE_DISCONNECTED 1
     )

ExternalProject_Add ( glog 
						DEPENDS gflags
					  	GIT_REPOSITORY ${ExternalProject_GIT_REPOSITORY}
						GIT_TAG ${ExternalProject_GIT_TAG}
						SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/glog
						CMAKE_ARGS ${GLOG_CMAKE_ARGS}
                    )

set(GLOG_INCLUDE_DIR ${CMAKE_INSTALL_PREFIX}/glog/include)
set(GLOG_LIBRARY_RELEASE ${CMAKE_INSTALL_PREFIX}/glog/lib/glog.lib)
set(GLOG_LIBRARY_DEBUG ${CMAKE_INSTALL_PREFIX}/glog/lib/glogd.lib)

set(ExternalProject_GIT_REPOSITORY "https://github.com/protocolbuffers/protobuf.git")
set(ExternalProject_GIT_TAG "main")
set (PROTOBUF_CMAKE_ARGS 
     -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}/protobuf # set the install prefix per generator     
     -DBUILD_SHARED_LIBS=OFF # build static libs  
     #-DCMAKE_DEBUG_POSTFIX=d # add a debug postfix to the debug libs so we can install them with the release ones
	 UPDATE_DISCONNECTED 1
     )


ExternalProject_Add ( protobuf
					  	GIT_REPOSITORY ${ExternalProject_GIT_REPOSITORY}
						GIT_TAG ${ExternalProject_GIT_TAG}                       
						SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/protobuf/vsprojects           
						CMAKE_ARGS ${PROTOBUF_CMAKE_ARGS}
                    )

set(PROTOBUF_SRC_ROOT_FOLDER ${CMAKE_INSTALL_PREFIX}/protobuf)

set(ExternalProject_GIT_REPOSITORY "https://github.com/google/leveldb.git")
set(ExternalProject_GIT_TAG "main")
set (LEVELDB_CMAKE_ARGS 
     -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}/leveldb # set the install prefix per generator     
     -DBUILD_SHARED_LIBS=OFF # build static libs  
     -DCMAKE_DEBUG_POSTFIX=d # add a debug postfix to the debug libs so we can install them with the release ones
     -DBOOST_ROOT=${Boost_ROOT}
     -DBOOST_INCLUDE_DIR=${Boost_INCLUDE_DIRS}
     -DBOOST_LIBRARYDIR=${Boost_LIBRARY_DIRS}
	 -DBoost_INCLUDE_DIR=${Boost_INCLUDE_DIR}
	 UPDATE_DISCONNECTED 1
     )


ExternalProject_Add ( leveldb
					  	GIT_REPOSITORY ${ExternalProject_GIT_REPOSITORY}
						GIT_TAG ${ExternalProject_GIT_TAG}                       
						SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/leveldb           
						CMAKE_ARGS ${LEVELDB_CMAKE_ARGS}
                    )

set(LEVELDB_ROOT ${CMAKE_INSTALL_PREFIX}/leveldb)

set (LMDB_CMAKE_ARGS 
     -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}/lmdb # set the install prefix per generator     
     -DBUILD_SHARED_LIBS=OFF # build static libs  
     #-DCMAKE_DEBUG_POSTFIX=d # add a debug postfix to the debug libs so we can install them with the release ones
	 UPDATE_DISCONNECTED 1
     )

set(ExternalProject_GIT_REPOSITORY "https://github.com/LMDB/lmdb.git")
set(ExternalProject_GIT_TAG "mdb.master")
ExternalProject_Add ( lmdb
					  	GIT_REPOSITORY ${ExternalProject_GIT_REPOSITORY}
						GIT_TAG ${ExternalProject_GIT_TAG}                       
						SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/lmdb/libraries/liblmdb        
						CMAKE_ARGS ${LMDB_CMAKE_ARGS}
                    )

# todo FIX debug optimized libraries for lmdb
set(LMDB_DIR ${CMAKE_INSTALL_PREFIX}/lmdb)


set(ExternalProject_GIT_REPOSITORY "https://github.com/google/snappy.git")
set(ExternalProject_GIT_TAG "main")
set (SNAPPY_CMAKE_ARGS 
     -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}/snappy # set the install prefix per generator     
     -DBUILD_SHARED_LIBS=OFF # build static libs  
     -DCMAKE_DEBUG_POSTFIX=d # add a debug postfix to the debug libs so we can install them with the release ones
	 UPDATE_DISCONNECTED 1
     )

ExternalProject_Add ( snappy
					  	GIT_REPOSITORY ${ExternalProject_GIT_REPOSITORY}
						GIT_TAG ${ExternalProject_GIT_TAG}                        
						SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/snappy        
						CMAKE_ARGS ${SNAPPY_CMAKE_ARGS}
                    )
# todo FIX debug optimized libraries for snappy
set(SNAPPY_ROOT_DIR ${CMAKE_INSTALL_PREFIX}/snappy)

add_subdirectory(opencv)
set(OpenCV_DIR ${CMAKE_INSTALL_PREFIX}/OpenCV)

add_subdirectory(hdf5)

set(HDF5_HL_INCLUDE_DIR          "${CMAKE_INSTALL_PREFIX}/hdf5/include"          CACHE PATH "hdf5 hl include dir")
set(HDF5_hdf5_LIBRARY_RELEASE    "${CMAKE_INSTALL_PREFIX}/hdf5/lib/libhdf5.lib"    CACHE PATH "hdf5 library")
set(HDF5_hdf5_hl_LIBRARY_RELEASE "${CMAKE_INSTALL_PREFIX}/hdf5/lib/libhdf5_hl.lib" CACHE PATH "hdf5 hl library")
set(HDF5_hdf5_LIBRARY_DEBUG      "${CMAKE_INSTALL_PREFIX}/hdf5/lib/libhdf5_D.lib"      CACHE PATH "hdf5 library")
set(HDF5_hdf5_hl_LIBRARY_DEBUG   "${CMAKE_INSTALL_PREFIX}/hdf5/lib/libhdf5_hl_D.lib"   CACHE PATH "hdf5 hl library")

set(OpenBLAS_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/openblas/include" CACHE PATH "OpenBLAS include dir")
set(OpenBLAS_LIB "${CMAKE_CURRENT_SOURCE_DIR}/openblas/lib/libopenblas.dll.a" CACHE FILEPATH "OpenBLAS lib")

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/caffe-cache-init.cmakein ${CMAKE_INSTALL_PREFIX}/caffe-cache-init.cmake @ONLY)



