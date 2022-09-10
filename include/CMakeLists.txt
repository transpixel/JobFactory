##
## -- CMake public header sources
##


# list of headers that form package public interface
set(thisProjPublicHeaders

	# TODO

	)

# install (public) header files
install(
	FILES ${thisProjPublicHeaders}
	DESTINATION
		${CMAKE_INSTALL_INCLUDEDIR}/${aProjName}
	)

