cmake_minimum_required(VERSION 2.6)

if (NOT CMAKE_BUILD_TYPE)
    set (CMAKE_BUILD_TYPE Release)
endif()

set (CMAKE_C_FLAGS "-Wall -Wextra -fPIE")
set (CMAKE_C_FLAGS_DEBUG "-g")
set (CMAKE_C_FLAGS_RELEASE "-O2")
set (CMAKE_CPP_FLAGS "-Wall -Wextra -fPIE")

project (iperf)

set (PACKAGE_VERSION 3.6)
set (PACKAGE_NAME "iperf")
set (PACKAGE_TARNAME "iperf")
set (PACKAGE_VERSION "3.6")
set (PACKAGE_STRING "iperf 3.6")
set (PACKAGE_BUGREPORT "https://github.com/esnet/iperf")
set (PACKAGE_URL "https://software.es.net/iperf/")
set (VERSION "3.6")
set (PACKAGE "iperf")

include(CheckIncludeFile)
include(CheckFunctionExists)
include(CheckTypeSize)

check_include_file("inttypes.h" HAVE_INTTYPES_H)
check_include_file("sys/cpuset.h" HAVE_CPUSET_SETAFFINITY)
check_include_file("sys/types.h" HAVE_SYS_TYPES_H)
check_include_file("sys/stat.h" HAVE_SYS_STAT_H)
check_include_file("stddef.h" STDC_HEADERS)
check_include_file("stdlib.h" HAVE_STDLIB_H)
check_include_file("memory.h" HAVE_MEMORY_H)
check_include_file("strings.h" HAVE_STRINGS_H)
check_include_file("string.h" HAVE_STRING_H)
check_include_file("stdint.h" HAVE_STDINT_H)
check_include_file("unistd.h" HAVE_UNISTD_H)
check_include_file("dlfcn.h" HAVE_DLFCN_H)
check_include_file("endian.h" HAVE_ENDIAN_H)
check_include_file("sys/endian.h" HAVE_SYS_ENDIAN_H)
check_include_file("netinet/sctp.h" HAVE_NETINET_SCTP_H)
check_include_file("netinet/sctp.h" HAVE_SCTP_H)
check_include_file("poll.h" HAVE_POLL_H)
check_include_file("openssl/ssl.h" HAVE_SSL)
check_include_file("sys/socket.h" HAVE_SYS_SOCKET_H)
check_include_file("sys/stat.h" HAVE_SYS_STAT_H)
check_function_exists("sched_setaffinity" HAVE_CPU_AFFINITY)
check_function_exists("sched_setaffinity" HAVE_SCHED_SETAFFINITY)
check_function_exists("daemon" HAVE_DAEMON)
check_function_exists("getline" HAVE_GETLINE)
check_function_exists("sendfile" HAVE_SENDFILE)
check_function_exists("SetProcessAffinityMask" HAVE_SETPROCESSAFFINITYMASK)
set(CMAKE_EXTRA_INCLUDE_FILES "netinet/sctp.h")
check_type_size("struct sctp_assoc_value" STRUCT_SCTP_ASSOC_VALUE)
set(CMAKE_EXTRA_INCLUDE_FILES)

configure_file ("${PROJECT_SOURCE_DIR}/src/version.h.in" "${PROJECT_BINARY_DIR}/version.h")
configure_file ("${PROJECT_SOURCE_DIR}/src/iperf_config.h.in" "${PROJECT_BINARY_DIR}/iperf_config.h")
include_directories(${PROJECT_SOURCE_DIR}/src ${PROJECT_BINARY_DIR})

add_executable(iperf
    src/main.c
    src/cjson.c
    src/cjson.h
    src/flowlabel.h
    src/iperf.h
    src/iperf_api.c
    src/iperf_api.h
    src/iperf_error.c
    src/iperf_auth.h
    src/iperf_auth.c
    src/iperf_client_api.c
    src/iperf_locale.c
    src/iperf_locale.h
    src/iperf_server_api.c
    src/iperf_tcp.c
    src/iperf_tcp.h
    src/iperf_udp.c
    src/iperf_udp.h
    src/iperf_sctp.c
    src/iperf_sctp.h
    src/iperf_util.c
    src/iperf_util.h
    src/dscp.c
    src/net.c
    src/net.h
    src/portable_endian.h
    src/queue.h
    src/tcp_info.c
    src/timer.c
    src/timer.h
    src/units.c
    src/units.h
    src/version.h
)

if (HAVE_SCTP_H)
    find_library(LIB_SCTP scp)
    target_link_libraries(iperf ${LIB_SCTP})
endif()
