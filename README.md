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
* CMakeLists.txt has been rewritten for static linking, and to fix several linking issues with libhl
* Changed linking of module.c and other required .c files from hl (exe) to libhl (lib now called hlcore)
* Non libhl/hlcore targets have been removed (out of scope of this project)
    * For now, this includes tests
