cmake_minimum_required(VERSION 3.0)

set(LZIP_VERSION_MAJOR 1)
set(LZIP_VERSION_MINOR 22)
set(PROGVERSION ${LZIP_VERSION_MAJOR}.${LZIP_VERSION_MINOR})

project(lzip VERSION ${PROGVERSION})

set(OBJS arg_parser.cc lzip_index.cc list.cc encoder_base.cc encoder.cc fast_encoder.cc decoder.cc main.cc)

include(CheckIncludeFiles)

CHECK_INCLUDE_FILES(unistd.h HAVE_UNISTD_H)
CHECK_INCLUDE_FILES(io.h HAVE_IO_H)
CHECK_INCLUDE_FILES(utime.h HAVE_UTIME_H)
CHECK_INCLUDE_FILES(sys/utime.h HAVE_SYS_UTIME_H)

configure_file(config.h.in ${CMAKE_SOURCE_DIR}/config.h)
if (MSVC)
    configure_file(lzip.rc.in ${CMAKE_SOURCE_DIR}/lzip.rc)
    set(LZIP_RC lzip.rc)
endif()

add_definitions(-DHAVE_CONFIG_H)

add_executable(lzip ${OBJS} ${LZIP_RC})

if (NOT CMAKE_INSTALL_BINDIR)
    set(CMAKE_INSTALL_BINDIR bin)
endif()

install(TARGETS lzip
RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
