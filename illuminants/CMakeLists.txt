cmake_minimum_required(VERSION 2.6)
project(Vole)

set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

include(VoleHelperMacros)
include(VoleModuleMacros)

if(WIN32)
	add_definitions(-DNOMINMAX) # prevents min- and max-macro brainf*ck in VS 2003
endif(WIN32)

# Global options
option(VOLE_CMAKE_DEBUG_OUTPUT "Show additonal cmake debug output." OFF)
mark_as_advanced(VOLE_CMAKE_DEBUG_OUTPUT)
option(VOLE_CHECK_DEPENDENCIES "Do additional dependency check with nice error messages." ON)
mark_as_advanced(VOLE_CHECK_DEPENDENCIES)
option(VOLE_AUTO_OPTIONAL_MODULES "Automatically link optional modules if possible." ON)
mark_as_advanced(VOLE_AUTO_OPTIONAL_MODULES)
option(VOLE_SINGLE_TARGETS "Build single targets for each command." OFF)
set(VOLE_MODULE_DIRECTORIES "" CACHE STRING "Semicolon-separated list of module directories for vole, e.g. the forensics or the color code.")

# Global variables
set(VOLE_LIBRARY_SUFFIX "-lib" CACHE INTERNAL "Module library suffix" FORCE)
set(VOLE_OPTIONAL_LIBRARY_SUFFIX "-optional-lib" CACHE INTERNAL "Module optional library suffix" FORCE)
set(VOLE_PACKAGE_LIST "" CACHE INTERNAL "Package list" FORCE)
set(VOLE_EXTERNAL_SUBDIRECTORIES "" CACHE INTERNAL "External subdirectories" FORCE)
set(VOLE_MODULE_LIST "" CACHE INTERNAL "Module list" FORCE)

include(VoleFindPackages)
include(VoleFindExternalModules)

include_directories(
	common
	storage
	shell
	rbase
	color_processing
	superpixels
	computational_geometry
	illumestimators/iic/
	illumestimators/iic/commands
	illumestimators/iic/misc
	illumestimators/iic/estimator
	illumestimators/iic/eval
	illumestimators/iic/mask
	illumestimators
	lille
)

add_subdirectory(common core/common)
add_subdirectory(storage core/storage)
add_subdirectory(rbase reflectance/rbase)
add_subdirectory(color_processing modules/color_processing)
add_subdirectory(superpixels modules/superpixels)
add_subdirectory(computational_geometry modules/computational_geometry)
add_subdirectory(illumestimators reflectance/illumestimators)
add_subdirectory(illumestimators/iic/commands reflectance/iic_commands)
add_subdirectory(illumestimators/iic/misc reflectance/iic_misc)
add_subdirectory(illumestimators/iic/estimator reflectance/iic_estimator)
add_subdirectory(illumestimators/iic/eval reflectance/iic_eval)
add_subdirectory(illumestimators/iic/mask reflectance/iic_mask)
add_subdirectory(lille reflectance/lille)
add_subdirectory(shell core/shell)

include(VoleCheckDependencies)
include(VoleProcessOptionalDependencies)


INCLUDE("CMakeModules/TargetDoc.cmake" OPTIONAL)

