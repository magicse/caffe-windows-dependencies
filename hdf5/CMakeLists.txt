set(extproject_name hdf5)
set(extproject_FILE "hdf5-1.10.4.zip")
set(extproject_URL "https://support.hdfgroup.org/ftp/HDF5/releases/hdf5-1.10/hdf5-1.10.4/src/${extproject_FILE}")

if(EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/${extproject_FILE}") 
	set(extproject_URL "${CMAKE_CURRENT_SOURCE_DIR}/${extproject_FILE}")
endif()

externalproject_add( ${extproject_name} 
					URL ${extproject_URL}							
					CMAKE_ARGS 
					-DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}/${extproject_name}
					-DBUILD_SHARED_LIBS=OFF
					-DBUILD_TESTING=OFF
					-HDF5_BUILD_EXAMPLES=OFF
					-HDF5_BUILD_TOOLS=OFF
					)
