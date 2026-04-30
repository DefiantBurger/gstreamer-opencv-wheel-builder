cmake_minimum_required(VERSION 3.24)
project(gstreamer_opencv_wheel_builder LANGUAGES C CXX)

find_package(Python3 REQUIRED COMPONENTS Interpreter Development.Module NumPy)

set(CV2_STAGING_DIR "/tmp/cv2_staging" CACHE PATH "Where cv2*.so was installed by before-all")

file(GLOB CV2_SO
    "${CV2_STAGING_DIR}/cv2*.so"
    "${CV2_STAGING_DIR}/cv2/python-*/cv2*.so"
)

if(NOT CV2_SO)
    message(FATAL_ERROR
        "Could not find cv2*.so under ${CV2_STAGING_DIR}. "
        "Check that the before-all step completed successfully.")
endif()

if(DEFINED SKBUILD_PLATLIB_DIR)
    set(INSTALL_DEST "${SKBUILD_PLATLIB_DIR}")
else()
    set(INSTALL_DEST "${Python3_SITELIB}")
endif()

foreach(SO_FILE ${CV2_SO})
    install(FILES "${SO_FILE}"
        DESTINATION "${INSTALL_DEST}"
        RENAME "cv2${CMAKE_SHARED_LIBRARY_SUFFIX}")
endforeach()