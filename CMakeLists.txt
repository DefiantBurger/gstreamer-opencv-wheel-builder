cmake_minimum_required(VERSION 3.24)

project(gstreamer_opencv_wheel_builder LANGUAGES NONE)

# The cv2 module was already built and installed by the before-all step.
# We copy it into the wheel package directory.

find_package(Python3 REQUIRED COMPONENTS Interpreter)

# Get Python's site-packages paths
execute_process(
    COMMAND ${Python3_EXECUTABLE} -c "import sysconfig; print(sysconfig.get_path('purelib'))"
    OUTPUT_VARIABLE PYTHON_PURELIB
    OUTPUT_STRIP_TRAILING_WHITESPACE
)

execute_process(
    COMMAND ${Python3_EXECUTABLE} -c "import site; print(site.getsitepackages()[0])"
    OUTPUT_VARIABLE PYTHON_SITE_PACKAGES
    OUTPUT_STRIP_TRAILING_WHITESPACE
    ERROR_QUIET
)

# Search for cv2 in common locations
find_path(CV2_LOCATION cv2 PATHS 
    "${PYTHON_PURELIB}"
    "${PYTHON_SITE_PACKAGES}"
    "/opt/python/cp312-cp312/lib/python3.12/site-packages"
    "/usr/local/lib/python3.12/site-packages"
    "/usr/lib/python3.12/site-packages"
    NO_DEFAULT_PATH
)

if(NOT CV2_LOCATION)
    message(FATAL_ERROR 
        "cv2 module not found.\n"
        "Searched:\n"
        "  ${PYTHON_PURELIB}\n"
        "  ${PYTHON_SITE_PACKAGES}\n"
        "OpenCV build in before-all may have failed.")
endif()

if(DEFINED SKBUILD_PLATLIB_DIR)
    set(WHEEL_PACKAGE_DIR "${SKBUILD_PLATLIB_DIR}")
else()
    set(WHEEL_PACKAGE_DIR "${CMAKE_INSTALL_PREFIX}")
endif()

# Copy the cv2 package into the wheel
file(COPY "${CV2_LOCATION}/cv2" DESTINATION "${WHEEL_PACKAGE_DIR}")
message(STATUS "Copied cv2 from ${CV2_LOCATION}/cv2 to ${WHEEL_PACKAGE_DIR}/cv2")
