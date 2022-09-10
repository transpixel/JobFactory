##
## -- CMake build system description
##

cmake_minimum_required(VERSION 3.13)

set(aProjName "JobFactory")
set(aProjVersion "1.0.0")

project(
	${aProjName}
	VERSION ${aProjVersion}
	DESCRIPTION "Job Factory for C++ parallel processing thread pools"
	LANGUAGES C CXX  # only C++ is used, but "C" for cmake to find pthreads
	)

message("### CMAKE_MAJOR_VERSION: " ${CMAKE_MAJOR_VERSION})
message("### CMAKE_MINOR_VERSION: " ${CMAKE_MINOR_VERSION})
message("### CMAKE_PATCH_VERSION: " ${CMAKE_PATCH_VERSION})
message("### CMAKE_TWEAK_VERSION: " ${CMAKE_TWEAK_VERSION})
message("### PROJECT: " ${PROJECT})
message("### PROJECT_VERSION: " ${PROJECT_VERSION})


set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# If any compile flags definitions - add here
add_definitions()


set(BUILD_FLAGS_FOR_CLANG
	""
	)
set(BUILD_FLAGS_FOR_GCC
	#
	# Disable
	#	https://gcc.gnu.org/bugzilla/show_bug.cgi?id=77728
	#	https://gcc.gnu.org/gcc-7/changes.html (ref "On ARM...")
	-Wno-psabi
	#
	-Wc++11-compat
	-pthread
	#
	-fomit-frame-pointer
	-fno-operator-names
	-fstrict-enums
	#
	-Wc++11-compat
	-Wall
	-Wextra
	-Wuninitialized
	-Winit-self
	-fno-nonansi-builtins
	-pedantic-errors
	#
	-fmax-errors=1  # keep template error message generation under control
	)
set(BUILD_FLAGS_FOR_VISUAL
	""
	)


#[[
# general configuration
# Note: can use target_compile_features() to customize for individual target
add_compile_options(
	-std=c++17
	-pedantic
	)
#]]

##
## -- Find dependency packages
##

find_package(Threads REQUIRED)

##
## -- configure packaging utilities
##

# unit test capabilities
# include and enable here for enable to be inherited by subdir tree
include(CTest)
enable_testing()

# Needs to be included first for use in subdir CMakeLists.txt files
include(GNUInstallDirs) # creates/sets CMAKE_INSTALL_<dir>


##
## -- project source tree description
##

set(EXPORT_ALIAS_NAME ${aProjName}::${aProjName})
set(EXPORT_TARGETS_NAME ${aProjName}Targets)
set(EXPORT_FILENAME ${EXPORT_TARGETS_NAME}.cmake)
message("### EXPORT_ALIAS_NAME: " ${EXPORT_ALIAS_NAME})
message("### EXPORT_TARGETS_NAME: " ${EXPORT_TARGETS_NAME})
message("### EXPORT_FILENAME: " ${EXPORT_FILENAME})


## -- Inform CMake about project tree

# Project source code
add_subdirectory(src) # project source code
add_subdirectory(include) # project library public interface (e.g. c++ headers)

# Documentation
add_subdirectory(doc) # project reference documentation

# Project test programs
add_subdirectory(tst) # developer unit tests (check source code operation)
#add_subdirectory(vnv) # use-case validation tests

# Project examples
#add_subdirectory(demos) # demonstration/example code

# Project applications
## add_subdirectory(apps) # application utility programs


# CMake configuration file packaging
add_subdirectory(cmake) # generate .cmake files for downstream use

##
## -- CMake configuration utilities
##

set_property(
	TARGET ${aProjName}
	PROPERTY VERSION ${thisProject_VERSION}
	)
set_property(
	TARGET ${aProjName}
	PROPERTY VERSION INTERFACE_thisProject_MAJOR_VERSION 1
	)
set_property(
	TARGET ${aProjName}
	APPEND PROPERTY COMPATIBLE_INTERFACE_STRING thisProject_MAJOR_VERSION
	)



##
## -- Distribution packages
##

# Override default install location for testing this package
# In general default (no command here) will install to /usr/{lib,bin,...}
# - but here, set package to sandbox location for easier/safer testing
# set (DEB_PACKAGE_ROOT "/tmpLocal/${aProjName}")
# - default is "/usr" (e.g. for system packages)
# - specify explicitly as "/usr/local" as a developer local package
# - extract from command line
set (DEB_PACKAGE_ROOT "${CMAKE_INSTALL_PREFIX}")
set(CPACK_PACKAGING_INSTALL_PREFIX "${DEB_PACKAGE_ROOT}")

# list of packages to create (e.g. "DEB;RPM;...") if all tools installed
set(CPACK_GENERATOR "DEB")


#
# === Distribution package creation
#

	set(thisProject_PACKAGE_VENDOR "https://host.domain.tld")
	set(thisProject_PACKAGE_MAINTAINER "https://host.domain.tld")

	# Specify generic cross-package distribuation properties
	# set(CPACK_PACKAGE_NAME ${aProjName}) # same as default
	set(CPACK_PACKAGE_VENDOR "${thisProject_PACKAGE_VENDOR}")
	set(CPACK_PACKAGE_VERSION_MAJOR ${PROJECT_VERSION_MAJOR})
	set(CPACK_PACKAGE_VERSION_MINOR ${PROJECT_VERSION_MINOR})
	set(CPACK_PACKAGE_VERSION_PATCH ${PROJECT_VERSION_PATCH})
	set(CPACK_PACKAGE_VERSION_TWEAK ${PROJECT_VERSION_TWEAK})


	# == Debian package

		set(CPACK_PACKAGING_INSTALL_PREFIX "${DEB_PACKAGE_ROOT}")
		# Set package(Generator) specific characteristics
		set(CPACK_DEBIAN_PACKAGE_MAINTAINER "${thisProject_PACKAGE_MAINTAINER}")
		# -- deps: none for thisProject
		#	set(APT_PACKAGE_DEPLIST
		#	"libeigen3-dev (>= ${EigenVersion}) \
		#	"
		#	)
		# set(CPACK_DEBIAN_PACKAGE_DEPENDS ${APT_PACKAGE_DEPLIST})


	# Must be included after the package variables are set
	include(CPack)

message("###")
message("### CMAKE_INSTALL_LIBDIR: " ${CMAKE_INSTALL_LIBDIR})
message("### CMAKE_INSTALL_BINDIR: " ${CMAKE_INSTALL_BINDIR})
message("### CMAKE_INSTALL_INCLUDEDIR: " ${CMAKE_INSTALL_INCLUDEDIR})
message("### CMAKE_INSTALL_DOCDIR: " ${CMAKE_INSTALL_DOCDIR})
message("###")
message("### CMAKE_INSTALL_FULL_LIBDIR: " ${CMAKE_INSTALL_FULL_LIBDIR})
message("### CMAKE_INSTALL_FULL_BINDIR: " ${CMAKE_INSTALL_FULL_BINDIR})
message("### CMAKE_INSTALL_FULL_INCLUDEDIR: " ${CMAKE_INSTALL_FULL_INCLUDEDIR})
message("### CMAKE_INSTALL_FULL_DOCDIR: " ${CMAKE_INSTALL_FULL_DOCDIR})
message("###")
message("### DEB_PACKAGE_ROOT: " ${DEB_PACKAGE_ROOT})
message("###")


