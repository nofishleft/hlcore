cmake_minimum_required(VERSION 3.5)

set(HL_VERSION_MAJOR 1)
set(HL_VERSION_MINOR 14)
set(HL_VERSION_PATCH 0)
set(HL_VERSION ${HL_VERSION_MAJOR}.${HL_VERSION_MINOR}.${HL_VERSION_PATCH})

project(hlcore VERSION ${HL_VERSION} LANGUAGES C)

option(HL_NO_THREADS "Force Single Threaded" ON)

if(WIN32)
    set(CMAKE_SYSTEM_VERSION 10.0)
endif()

file(GLOB pcre_srcs
        vendor/pcre/pcre2_auto_possess.c
        vendor/pcre/pcre2_chartables.c
        vendor/pcre/pcre2_compile.c
        vendor/pcre/pcre2_config.c
        vendor/pcre/pcre2_context.c
        vendor/pcre/pcre2_convert.c
        vendor/pcre/pcre2_dfa_match.c
        vendor/pcre/pcre2_error.c
        vendor/pcre/pcre2_extuni.c
        vendor/pcre/pcre2_find_bracket.c
        vendor/pcre/pcre2_jit_compile.c
        vendor/pcre/pcre2_maketables.c
        vendor/pcre/pcre2_match_data.c
        vendor/pcre/pcre2_match.c
        vendor/pcre/pcre2_newline.c
        vendor/pcre/pcre2_ord2utf.c
        vendor/pcre/pcre2_pattern_info.c
        vendor/pcre/pcre2_script_run.c
        vendor/pcre/pcre2_serialize.c
        vendor/pcre/pcre2_string_utils.c
        vendor/pcre/pcre2_study.c
        vendor/pcre/pcre2_substitute.c
        vendor/pcre/pcre2_substring.c
        vendor/pcre/pcre2_tables.c
        vendor/pcre/pcre2_ucd.c
        vendor/pcre/pcre2_valid_utf.c
        vendor/pcre/pcre2_xclass.c
)

file(GLOB std_srcs
        src/std/array.c
        src/std/buffer.c
        src/std/bytes.c
        src/std/cast.c
        src/std/date.c
        src/std/debug.c
        src/std/error.c
        src/std/file.c
        src/std/fun.c
        src/std/maps.c
        src/std/math.c
        src/std/obj.c
        src/std/random.c
        src/std/regexp.c
        src/std/socket.c
        src/std/string.c
        src/std/sys.c
        src/std/track.c
        src/std/types.c
        src/std/ucs2.c
        src/std/thread.c
        src/std/process.c
)

if(ANDROID)
    list(APPEND std_srcs
            src/std/sys_android.c
    )
endif()

if(IOS_PLATFORM)
    list(APPEND std_srcs
            src/std/sys_ios.m
    )
endif()

file(GLOB hlcore_srcs
        src/gc.c
        src/code.c
        src/jit.c
        src/module.c
        src/debugger.c
        src/profile.c
)

set(hlcore_private_include
        "vendor/pcre"
        "src"
)

set(hlcore_public_include "include")

set(hlcore_private_definitions "HAVE_CONFIG_H" "PCRE2_CODE_UNIT_WIDTH=16")

set(hlcore_public_definitions "")

if (HL_NO_THREADS STREQUAL "ON")
    list(APPEND hlcore_public_definitions "HL_NO_THREADS")
endif()

if(MSVC)
    # force Unicode over Multi-byte
    list(APPEND hlcore_public_definitions "UNICODE" "_UNICODE")
endif()

if(WIN32)
    set(hlcore_libraries "ws2_32" "user32")
else()
    set(hlcore_libraries "m" "dl" "pthread")
endif()

add_library(hlcore STATIC
        ${pcre_srcs}
        ${hlcore_srcs}
        ${std_srcs}
)

target_include_directories(hlcore
        PRIVATE ${hlcore_private_include}
        PUBLIC ${hlcore_public_include}
)

target_compile_definitions(hlcore
        PRIVATE ${hlcore_private_definitions}
        PUBLIC ${hlcore_public_definitions}
)

target_link_libraries(hlcore ${hlcore_libraries})

set_property(TARGET hlcore PROPERTY C_STANDARD 11)
set_property(TARGET hlcore PROPERTY C_STANDARD_REQUIRED ON)

#set_target_properties(hlcore
#    PROPERTIES
#    VERSION ${HL_VERSION}
#    SOVERSION ${HL_VERSION_MAJOR}
#    COMPILE_DEFINITIONS "_USRDLL;LIBHL_EXPORTS;HAVE_CONFIG_H;PCRE2_CODE_UNIT_WIDTH=16"
#)
