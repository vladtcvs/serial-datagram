cmake_minimum_required(VERSION 3.13)

set(DEST_DIR "${CMAKE_INSTALL_PREFIX}")

include(FindPkgConfig)
pkg_search_module(CRC32 REQUIRED crc32)

add_library(serial_datagram STATIC serial_datagram.c)
target_include_directories(serial_datagram PUBLIC .)

target_include_directories(serial_datagram SYSTEM PUBLIC ${CRC32_INCLUDE_DIRS})
target_link_directories(serial_datagram PUBLIC ${CRC32_LIBRARY_DIRS})
target_link_libraries(serial_datagram ${CRC32_LIBRARIES})

CONFIGURE_FILE("serial_datagram.pc.in" "serial_datagram.pc" @ONLY)

set(RT ${CMAKE_SOURCE_DIR})

install(TARGETS serial_datagram ARCHIVE DESTINATION lib)
install(FILES   ${RT}/serial_datagram.h
  DESTINATION include/serial-datagram)
install(FILES ${CMAKE_BINARY_DIR}/serial_datagram.pc
	DESTINATION lib/pkgconfig)
