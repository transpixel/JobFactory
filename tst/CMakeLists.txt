
# Inventory of test programs
set(thisProjTests

	# TODO

	)

# build exectutables
foreach (thisProjTest ${thisProjTests})

	# each test is independent executable program
	add_executable(${thisProjTest} ${thisProjTest}.cpp)

	# use may project build options
	target_compile_options(
		${thisProjTest}
		PRIVATE
			$<$<CXX_COMPILER_ID:Clang>:${BUILD_FLAGS_FOR_CLANG}>
			$<$<CXX_COMPILER_ID:GNU>:${BUILD_FLAGS_FOR_GCC}>
			$<$<CXX_COMPILER_ID:MSVC>:${BUILD_FLAGS_FOR_VISUAL}>
		)

	# local and project include paths
	target_include_directories(
		${thisProjTest}
		PRIVATE
			$<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}>/
			$<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}>/../src/
			$<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}>/../include/
        	$<INSTALL_INTERFACE:include/>
		)

	# dependency on project
	target_link_libraries(
		${thisProjTest}
		PRIVATE
		${aProjName}::${aProjName}
			engabra::engabra
		)

endforeach()

# add to "CTest" suite
foreach (thisProjTest ${thisProjTests})

	add_test("${thisProjTest}"
		"${thisProjTest}"
		)

endforeach ()


