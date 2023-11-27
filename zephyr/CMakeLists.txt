# SPDX-License-Identifier: Apache-2.0
if (CONFIG_CYCLONEDDS_CDR)

set(CMAKE_MODULE_PATH "${ZEPHYR_CURRENT_MODULE_DIR}/cmake/Modules")
include(Generate)
include(GenerateExportHeader)

zephyr_library_named(cdr)
#zephyr_library_compile_options(cdr -Wno-type-limits)
zephyr_library_include_directories(
	${CMAKE_CURRENT_BINARY_DIR}/include
	"${ZEPHYR_CURRENT_MODULE_DIR}/src/ddsrt/include"
	"${ZEPHYR_CURRENT_MODULE_DIR}/src/core/cdr/include"
	"${ZEPHYR_CURRENT_MODULE_DIR}/src/core/ddsc/include"
	"${ZEPHYR_CURRENT_MODULE_DIR}/zephyr/src"
	)
zephyr_library_sources(
	"${ZEPHYR_CURRENT_MODULE_DIR}/src/core/cdr/src/dds_cdrstream.c"
	)

zephyr_include_directories(
	${CMAKE_CURRENT_BINARY_DIR}/include
	"${ZEPHYR_CURRENT_MODULE_DIR}/src/ddsrt/include"
	"${ZEPHYR_CURRENT_MODULE_DIR}/src/core/cdr/include"
	"${ZEPHYR_CURRENT_MODULE_DIR}/src/core/ddsc/include"
	"${ZEPHYR_CURRENT_MODULE_DIR}/zephyr/src"
	)

configure_file(${ZEPHYR_CURRENT_MODULE_DIR}/src/ddsrt/include/dds/config.h.in include/dds/config.h)
configure_file(${ZEPHYR_CURRENT_MODULE_DIR}/src/ddsrt/include/dds/features.h.in include/dds/features.h)
configure_file(${ZEPHYR_CURRENT_MODULE_DIR}/src/ddsrt/include/dds/version.h.in include/dds/version.h)
generate_export_header(cdr BASE_NAME DDS EXPORT_FILE_NAME include/dds/export.h)

add_dependencies(app cdr)

endif()

if (CONFIG_CYCLONEDDS_IDLC)

include(ExternalProject)

ExternalProject_Add(idlc
  SOURCE_DIR ${ZEPHYR_CURRENT_MODULE_DIR}
  PREFIX ${CMAKE_CURRENT_BINARY_DIR}
  CONFIGURE_COMMAND ${CMAKE_COMMAND} ${ZEPHYR_CURRENT_MODULE_DIR}
        -DBUILD_DDSPERF=OFF
        -DBUILD_EXAMPLES=OFF
	-DENABLE_ICEORYX=NO
	-DCMAKE_BUILD_TYPE=Release
	-DCMAKE_INSTALL_PREFIX=${CMAKE_CURRENT_BINARY_DIR}
)

add_dependencies(app idlc)

endif()
