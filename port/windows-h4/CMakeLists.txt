cmake_minimum_required (VERSION 3.5)

project(BTstack-windows-h4)

# extra compiler warnings
if ("${CMAKE_C_COMPILER_ID}" MATCHES ".*Clang.*")
	# using Clang
	SET(CMAKE_C_FLAGS  "${CMAKE_C_FLAGS} -Wunused-variable -Wswitch-default -Werror")
elseif ("${CMAKE_C_COMPILER_ID}" STREQUAL "GNU")
	# using GCC
	SET(CMAKE_C_FLAGS  "${CMAKE_C_FLAGS} -Wunused-but-set-variable -Wunused-variable -Wswitch-default -Werror")
elseif ("${CMAKE_C_COMPILER_ID}" STREQUAL "Intel")
	# using Intel C++
elseif ("${CMAKE_C_COMPILER_ID}" STREQUAL "MSVC")
	# using Visual Studio C++
endif()

# to find generated .h from .gatt files
include_directories(${CMAKE_CURRENT_BINARY_DIR})

# local dir for btstack_config.h after build dir to avoid using .h from Makefile
include_directories(.)


include_directories(../../3rd-party/micro-ecc)
include_directories(../../3rd-party/bluedroid/decoder/include)
include_directories(../../3rd-party/bluedroid/encoder/include)
include_directories(../../3rd-party/lc3-google/include)
include_directories(../../3rd-party/md5)
include_directories(../../3rd-party/hxcmod-player)
include_directories(../../3rd-party/hxcmod-player/mod)
include_directories(../../3rd-party/lwip/core/src/include)
include_directories(../../3rd-party/lwip/dhcp-server)
include_directories(../../3rd-party/rijndael)
include_directories(../../3rd-party/yxml)
include_directories(../../3rd-party/tinydir)
include_directories(../../src)
include_directories(../../chipset/bcm)
include_directories(../../chipset/csr)
include_directories(../../chipset/cc256x)
include_directories(../../chipset/em9301)
include_directories(../../chipset/realtek)
include_directories(../../chipset/tc3566x)
include_directories(../../chipset/stlc2500d)
include_directories(../../chipset/zephyr)
include_directories(../../platform/embedded)
include_directories(../../platform/lwip)
include_directories(../../platform/lwip/port)
include_directories(../../platform/windows)

file(GLOB SOURCES_SRC       "../../src/*.c")
file(GLOB SOURCES_BLE       "../../src/ble/*.c")
file(GLOB SOURCES_BLUEDROID "../../3rd-party/bluedroid/encoder/srce/*.c" "../../3rd-party/bluedroid/decoder/srce/*.c")
file(GLOB SOURCES_CLASSIC   "../../src/classic/*.c")
file(GLOB SOURCES_MESH      "../../src/mesh/*.c")
file(GLOB SOURCES_GATT      "../../src/ble/gatt-service/*.c")
file(GLOB SOURCES_UECC      "../../3rd-party/micro-ecc/uECC.c")
file(GLOB SOURCES_HXCMOD    "../../3rd-party/hxcmod-player/*.c"  "../../3rd-party/hxcmod-player/mods/*.c")
file(GLOB SOURCES_RIJNDAEL  "../../3rd-party/rijndael/rijndael.c")
file(GLOB SOURCES_YXML      "../../3rd-party/yxml/yxml.c")
file(GLOB SOURCES_WINDOWS   "../../platform/windows/*.c")
file(GLOB SOURCES_BCM       "../../chipset/bcm/*.c")
file(GLOB SOURCES_CSR       "../../chipset/csr/*.c")
file(GLOB SOURCES_EM9301    "../../chipset/em9301/*.c")
file(GLOB SOURCES_STLC2500D "../../chipset/stlc2500d/*.c")
file(GLOB SOURCES_TC2566X   "../../chipset/tc3566x/*.c")
file(GLOB SOURCES_REALTEK   "../../chipset/realtek/*.c")
file(GLOB SOURCES_ZEPHYR    "../../chipset/zephyr/*.c")
file(GLOB SOURCES_LC3_GOOGLE "../../3rd-party/lc3-google/src/*.c")
file(GLOB SOURCES_PORT      "*.c")

set(LWIP_CORE_SRC
		../../3rd-party/lwip/core/src/core/def.c
		../../3rd-party/lwip/core/src/core/inet_chksum.c
		../../3rd-party/lwip/core/src/core/init.c
		../../3rd-party/lwip/core/src/core/ip.c
		../../3rd-party/lwip/core/src/core/mem.c
		../../3rd-party/lwip/core/src/core/memp.c
		../../3rd-party/lwip/core/src/core/netif.c
		../../3rd-party/lwip/core/src/core/pbuf.c
		../../3rd-party/lwip/core/src/core/tcp.c
		../../3rd-party/lwip/core/src/core/tcp_in.c
		../../3rd-party/lwip/core/src/core/tcp_out.c
		../../3rd-party/lwip/core/src/core/timeouts.c
		../../3rd-party/lwip/core/src/core/udp.c
		)
set (LWIP_IPV4_SRC
		../../3rd-party/lwip/core/src/core/ipv4/acd.c
		../../3rd-party/lwip/core/src/core/ipv4/dhcp.c
		../../3rd-party/lwip/core/src/core/ipv4/etharp.c
		../../3rd-party/lwip/core/src/core/ipv4/icmp.c
		../../3rd-party/lwip/core/src/core/ipv4/ip4.c
		../../3rd-party/lwip/core/src/core/ipv4/ip4_addr.c
		../../3rd-party/lwip/core/src/core/ipv4/ip4_frag.c
		)
set (LWIP_NETIF_SRC
		../../3rd-party/lwip/core/src/netif/ethernet.c
		)
set (LWIP_HTTPD
		../../3rd-party/lwip/core/src/apps/http/altcp_proxyconnect.c
		../../3rd-party/lwip/core/src/apps/http/fs.c
		../../3rd-party/lwip/core/src/apps/http/httpd.c
		)
set (LWIP_DHCPD
		../../3rd-party/lwip/dhcp-server/dhserver.c
		)
set (LWIP_PORT
		../../platform/lwip/port/sys_arch.c
		../../platform/lwip/bnep_lwip.c
		)
set (SOURCES_LWIP ${LWIP_CORE_SRC} ${LWIP_IPV4_SRC} ${LWIP_NETIF_SRC} ${LWIP_HTTPD} ${LWIP_DHCPD} ${LWIP_PORT})

file(GLOB SOURCES_BLE_OFF "../../src/ble/le_device_db_memory.c")
list(REMOVE_ITEM SOURCES_BLE   ${SOURCES_BLE_OFF})

file(GLOB SOURCES_WINDOWS_OFF "../../platform/windows/hci_transport_h2_winusb.c")
list(REMOVE_ITEM SOURCES_WINDOWS   ${SOURCES_WINDOWS_OFF})

set(SOURCES
		${SOURCES_BLE}
		${SOURCES_BCM}
		${SOURCES_BLUEDROID}
		${SOURCES_CLASSIC}
		${SOURCES_CSR}
		${SOURCES_EM9301}
		${SOURCES_GATT}
		${SOURCES_HXCMOD}
		${SOURCES_HXCMOD}
		${SOURCES_LIBUSB}
		${SOURCES_MD5}
		${SOURCES_MESH}
		${SOURCES_PORT}
		${SOURCES_REALTEK}
		${SOURCES_RIJNDAEL}
		${SOURCES_SRC}
		${SOURCES_STLC2500D}
		${SOURCES_TC2566X}
		${SOURCES_UECC}
		${SOURCES_WINDOWS}
		${SOURCES_YXML}
		${SOURCES_ZEPHYR}
)
list(SORT SOURCES)

# create static lib
add_library(btstack STATIC ${SOURCES})

# get list of examples, skipping mesh_node_demo
include(../../example/CMakeLists.txt)
set (EXAMPLES ${EXAMPLES_GENERAL} ${EXAMPLES_CLASSIC_ONLY}  ${EXAMPLES_LE_ONLY} ${EXAMPLES_DUAL_MODE})
list(REMOVE_DUPLICATES EXAMPLES)
list(REMOVE_ITEM EXAMPLES "mesh_node_demo")

# create targets
foreach(EXAMPLE ${EXAMPLES})
	# get c file
	file(GLOB EXAMPLE_FILE "../../example/${EXAMPLE}.c")

	# add GATT DB creation
	if ( "${EXAMPLES_GATT_FILES}" MATCHES ${EXAMPLE} )
		message("example ${EXAMPLE} -- with GATT DB")
		add_custom_command(
		    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/${EXAMPLE}.h
			DEPENDS ${CMAKE_SOURCE_DIR}/../../example/${EXAMPLE}.gatt
			COMMAND ${CMAKE_SOURCE_DIR}/../../tool/compile_gatt.py
			ARGS ${CMAKE_SOURCE_DIR}/../../example/${EXAMPLE}.gatt ${CMAKE_CURRENT_BINARY_DIR}/${EXAMPLE}.h
		)
		list(APPEND SOURCE_FILES ${CMAKE_CURRENT_BINARY_DIR}/${EXAMPLE}.h)
	else()
		message("example ${EXAMPLE}")
	endif()
	add_executable(${EXAMPLE} ${EXAMPLE_FILE} )
	target_link_libraries(${EXAMPLE} btstack)
endforeach(EXAMPLE)
