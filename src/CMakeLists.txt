##
## -- CMake source code tree
##


set(theProjLibName ${aProjName})

# list of sources on which library depends
set(theProjLibSources

	jobCapacity.cpp
	jobFactory.cpp
	Timer.cpp
	Utilization.cpp

	)

#[[
# list of headers that form package public interface
set(theProjPublicHeaders

	# TODO

	)
# install (public) header files
install(
	FILES ${theProjPublicHeaders}
	DESTINATION
		${CMAKE_INSTALL_INCLUDEDIR}/${aProjName}
	)
#]]


# create CMake library target
add_library(
	${theProjLibName}
	STATIC
	${theProjLibSources}
	)

target_include_directories(
	${theProjLibName}
	PUBLIC
		$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/../include>
		$<INSTALL_INTERFACE:include/${aProjName}>
	PRIVATE
		${CMAKE_CURRENT_SOURCE_DIR}
	)

target_link_libraries(
	${theProjLibName}
	)

# install the library
install(
	TARGETS ${theProjLibName}
	ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
	)

# Alias for use in downstream CMakeLists
add_library(${aProjName}::${aProjName} ALIAS ${theProjLibName})

# Export info
add_library(
	${EXPORT_ALIAS_NAME}
	ALIAS
	${theProjLibName}
	)

# export linkable library target
install(
	TARGETS ${theProjLibName}
	EXPORT ${EXPORT_TARGETS_NAME}
	ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
	#[[
	RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
	INCLUDES DESTINATION
		$<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}/${theProj_LibDIR}>
		$<INSTALL_INTERFACE:${theProj_LibDIR}>
	NAMESPACE ${aProjName}:: # e.g. thisProj::thisProj
	#]]
	)

