# This source file is part of the Swift.org open source project
#
# Copyright (c) 2014 - 2019 Apple Inc. and the Swift project authors
# Licensed under Apache License v2.0 with Runtime Library Exception
#
# See http://swift.org/LICENSE.txt for license information
# See http://swift.org/CONTRIBUTORS.txt for Swift project authors

cmake_minimum_required(VERSION 3.15.1)

project(example LANGUAGES C Swift)
option(USE_VENDORED_TSC "Use vendored version of TSC" OFF)

set(SWIFT_VERSION 5)
set(CMAKE_Swift_LANGUAGE_VERSION ${SWIFT_VERSION})
if(CMAKE_VERSION VERSION_LESS 3.16)
    add_compile_options($<$<COMPILE_LANGUAGE:Swift>:-swift-version$<SEMICOLON>${SWIFT_VERSION}>)
endif()

set(CMAKE_Swift_MODULE_DIRECTORY ${CMAKE_BINARY_DIR}/swift)

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

option(BUILD_SHARED_LIBS "Build shared libraryes by default" YES)

if(USE_VENDORED_TSC)
    message("Using vendored TSC")
else()
    message("Not using vendored TSC")
    find_package(TSC CONFIG REQUIRED)
endif()

if(LLBUILD_FRAMEWORK)
    message("Using llbuild framework: ${LLBUILD_FRAMEWORK}")
else()
    message("Using libllbuild")
    find_package(LLBuild CONFIG REQUIRED)
endif()

add_subdirectory(Sources)

if(SWIFTPM_BUILD_DIR)
    add_subdirectory(Utilities/BuildExtras/Install)
endif()
