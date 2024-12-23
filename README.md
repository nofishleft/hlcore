# HLCore

A fork of HashLink, containing the core functionality of libhl (part of the HashLink virtual machine), for embedding inside a game or game engine.

Currently tracking [HashLink 1.14](https://github.com/HaxeFoundation/hashlink/tree/1.14).

## Changes compared to HashLink

* Vendor libraries in /libs/ have been removed (fmt, openal, sdl, ssl, uv, sqlite, etc.)
* All vendored libraries except for pcre in /include/ have been removed
* Renamed /include/ to /vendor/
* Moved public headers from /src/ to /include/
    * Private headers stayed in /src/
* HL_NO_THREADS has been exposed as a CMake option, allowing explicit single threading of the vm, without having to modify HashLink source code.
* CMakeLists.txt has been rewritten to support FetchContent and static linking, and to fix several linking issues with libhl
* Changed linking of module.c and other required .c files from hl (exe) to libhl (lib now called hlcore)
* Non libhl/hlcore targets have been removed (out of scope of this project)
    * For now, this includes tests

## Usage

### CMake via [CPM.cmake](https://github.com/cpm-cmake/CPM.cmake)

```cmake
include(get_cpm.cmake)

CPMAddPackage(
        NAME hlcore
        VERSION 1.14
        URL <HLCORE_URL>
        URL_HASH SHA256=<HLCORE_HASH>
        OPTIONS
        # Force single-threaded, set to OFF to enable multi-threading
        "HL_NO_THREADS ON"
)

add_executable(my_target ...)

target_link_libraries(my_target nofishleft::hlcore)
```

| | |
| --- | --- |
| URL | Link to a source archive of desired commit/tag/branch/release. |
| URL_HASH | A checksum to verify that the source hasn't changed. Recommended if not linking to a specific commit. Can be generated from [emn178/online-tools/sha256_checksum](https://emn178.github.io/online-tools/sha256_checksum.html). |
