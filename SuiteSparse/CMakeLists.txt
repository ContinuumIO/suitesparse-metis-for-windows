PROJECT(SuiteSparse)

# Disable COMPLEX numbers: disable it by default, since it causes problems in some platforms.
SET(HAVE_COMPLEX OFF CACHE BOOL "Enables building SuiteSparse with complex numbers (disabled by default to avoid problems in some platforms)")
IF (NOT HAVE_COMPLEX)
	ADD_DEFINITIONS(-DNCOMPLEX)
ENDIF (NOT HAVE_COMPLEX)

# Used even for MSVC, so blas function names have the final "_"
if (MSVC)
	ADD_DEFINITIONS(-D__MINGW32__)
endif(MSVC)


include_directories(".")  # Needed for "SourceWrappers/*.c" files

set(suitesparseconfig_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/SuiteSparse_config) # Needed by all subprojects

set(SUBPROJECTS_TO_ADD 
	SuiteSparse_config
	AMD
	BTF
	CAMD
	CCOLAMD
	COLAMD
	CHOLMOD
	CXSparse
	KLU
	LDL
	UMFPACK
	SPQR
	CACHE STRING "list of all sub-directories to add to cmake build"
)

if(WITH_CUDA)
	set(SUBPROJECTS_TO_ADD
		${SUBPROJECTS_TO_ADD}
		SuiteSparse_GPURuntime
		GPUQREngine	
		)
endif()

foreach(subdir ${SUBPROJECTS_TO_ADD})
	add_subdirectory(${subdir})
endforeach()
