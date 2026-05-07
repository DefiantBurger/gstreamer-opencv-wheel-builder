# gstreamer-opencv-wheel-builder

Builds OpenCV wheels with GStreamer support in four release variants:

| Architecture | Variant | CI job | Release tag | Local target |
| --- | --- | --- | --- | --- |
| x86_64 | headed | `build-x86-headed` | `latest-x86_64-headed` | `make prebuilt-headed-x86_64` |
| x86_64 | headless | `build-x86-headless` | `latest-x86_64-headless` | `make prebuilt-headless-x86_64` |
| arm64 | headed | `build-arm64-headed` | `latest-arm64-headed` | `make prebuilt-headed-arm64` |
| arm64 | headless | `build-arm64-headless` | `latest-arm64-headless` | `make prebuilt-headless-arm64` |

## Local builds

The `source-build-headed` and `source-build-headless` targets build OpenCV from source on the current machine architecture.
Use the `prebuilt-*` targets above to install the latest wheel published by CI for a specific architecture and variant.

## CI output

The GitHub Actions workflow publishes the matching wheel for each matrix cell and smoke-tests it with `cv2.getBuildInformation()` to confirm GStreamer support.

Headed wheels are named `opencv_python-*.whl`, and headless wheels are named `opencv_python_headless-*.whl`.
