project(codal-samd21-mbed)

include(../../utils/cmake/util.cmake)

# find sources and headers
RECURSIVE_FIND_DIR(INCLUDE_DIRS "${CMAKE_CURRENT_SOURCE_DIR}/${codal.target.device}" "*.h")
RECURSIVE_FIND_FILE(SOURCE_FILES "${CMAKE_CURRENT_SOURCE_DIR}/${codal.target.device}" "*.c??")

# force include conf_clocks...
set(CMAKE_C_FLAGS      "${CMAKE_C_FLAGS} -imacros \"${CMAKE_CURRENT_SOURCE_DIR}/conf_clocks.h\"" PARENT_SCOPE)
set(CMAKE_CXX_FLAGS    "${CMAKE_CXX_FLAGS} -imacros \"${CMAKE_CURRENT_SOURCE_DIR}/conf_clocks.h\"" PARENT_SCOPE)

# add them
include_directories("${INCLUDE_DIRS}")

# create our target
add_library(codal-samd21-mbed "${SOURCE_FILES}")

target_link_libraries(
    codal-samd21-mbed
    codal
    mbed-classic
    codal-mbed
)

# expose it to parent cmake.
target_include_directories(codal-samd21-mbed PUBLIC ${INCLUDE_DIRS} "${CMAKE_CURRENT_SOURCE_DIR}/")

if(CODAL_HEADER_EXTRACTION)
    RECURSIVE_FIND_FILE(INCLUDE_FILES "${CMAKE_CURRENT_SOURCE_DIR}/${codal.target.device}" "*.h")
    HEADER_FILES(INCLUDE_FILES2 ".")

    file(COPY ${INCLUDE_FILES} DESTINATION "../../build/codal-samd21-mbed/includes")
    file(COPY ${INCLUDE_FILES2} DESTINATION "../../build/codal-samd21-mbed/includes")
endif()