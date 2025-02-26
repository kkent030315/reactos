
list(APPEND SOURCE
    src/ansiprsr.cpp
    src/keytrans.cpp
    src/tcharmap.cpp
    src/tconsole.cpp
    src/tkeydef.cpp
    src/tkeymap.cpp
    src/tmapldr.cpp
    src/tmouse.cpp
    src/tnclass.cpp
    src/tnclip.cpp
    src/tncon.cpp
    src/tnconfig.cpp
    src/tnerror.cpp
    src/tnetwork.cpp
    src/tnmain.cpp
    src/tnmisc.cpp
    src/tscript.cpp
    src/tscroll.cpp
    src/ttelhndl.cpp
    precomp.h)

add_executable(telnet ${SOURCE} telnet.rc)
target_link_libraries(telnet cppstl)
set_target_cpp_properties(telnet WITH_EXCEPTIONS)

if (MSVC)
    # C4838: conversion from 'int' to 'SHORT' requires a narrowing conversion
    # C4996: 'strnicmp': Deprecated POSIX name, Try _strnicmp instead!
    target_compile_options(telnet PRIVATE /wd4838 /wd4996)
endif()

if (NOT MSVC)
    target_compile_definitions(telnet PRIVATE _CRT_NONSTDC_NO_DEPRECATE)
endif()

if(CMAKE_C_COMPILER_ID STREQUAL "GNU")
    target_compile_options(telnet PRIVATE -Wno-restrict $<$<COMPILE_LANGUAGE:CXX>:-Wno-narrowing>)
endif()

set_module_type(telnet win32cui)
add_importlibs(telnet ws2_32 user32 msvcrt kernel32 ntdll)
add_pch(telnet precomp.h SOURCE)
add_cd_file(TARGET telnet DESTINATION reactos/system32 FOR all)
