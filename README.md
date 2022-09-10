
# JobFactory -- Job Factory for C++ parallel processing thread pools

Utility library that provides a base class (JobBase) for defining an arbitrary
processing job in a wrapper that is compatible with the parallel processing
thread pool (JobFactory).

## Development Operations

Commands below refer to following directories

- ${srcLibDir} -- project source tree (e.g. including this README.md file) (e.g. .../MyGitRepos/JobFactory).

- ${buildLibDir} -- transitory root directory for (out of source) build tree
	(e.g. /tmp/JobFactoryBuildTree)

- ${instLibDir} -- directory in which to install (e.g. /usr/local)


Generating native build system

	cmake \
		-DCMAKE_INSTALL_PREFIX=${instLibDir} \
		-DCMAKE_INSTALL_MESSAGE=NEVER \
		-DBUILD_SHARED_LIBS=NO \
		-B${buildLibDir} \
		-H${srcLibDir} \
		;

Building with native toolset

	cmake \
		--build ${buildLibDir} \
		--config Release \
		--clean-first \
		-- -j `nproc` \
		;

Installing

	cmake \
		--build ${buildLibDir} \
		--config Release \
		--target install \
		;

Running Tests

	cmake \
		--build ${buildLibDir} \
		--target test \
		;


Making Package

	cmake \
		--build ${buildLibDir} \
		--target package \
		;


Package Info

	ls -l ${buildLibDir}/*.deb
	dpkg -f ${buildLibDir}/TPQZcam*.deb

