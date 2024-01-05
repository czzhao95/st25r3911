add_definitions(-DST25R3911)

if( CARD_EMULATION_ONLY )
    message("ERROR: Card Emulation not supported by " ${RFAL_VARIANT})
    return()
endif()

# RFAL uses folder "st25r3911" for "st25r3911b"
set(RFAL_VARIANT_FAMILY "st25r3911" PARENT_SCOPE)
set(RFAL_VARIANT_FAMILY "st25r3911")

# Bring headers into the project
include_directories(
    ../../../common/firmware/STM/utils/Inc
    ../../../rfal/include
    ../../../rfal/source/${RFAL_VARIANT_FAMILY}
    ../../../rfal/source
    ../Inc
    ../${RFAL_VARIANT}
    )

# Source files to build the project
file(GLOB SOURCE1   "../../../rfal/source/*.c")
file(GLOB SOURCE2   "../../../rfal/source/${RFAL_VARIANT_FAMILY}/*.c")

file(GLOB PLATFORM1 "../../platform/Src/pltf_gpio.c")
file(GLOB PLATFORM2 "../../platform/Src/pltf_timer.c")
file(GLOB PLATFORM3 "../../platform/Src/pltf_spi.c")

set(SOURCE ${SOURCE1} ${SOURCE2} ${PLATFORM1} ${PLATFORM2} ${PLATFORM3})

# Link with rt for POSIX timers
find_library(LIBRT_PATH rt)

# Create the shared library
add_library(rfal_${RFAL_VARIANT} SHARED ${SOURCE})

target_link_libraries(rfal_${RFAL_VARIANT} "${LIBRT_PATH}")
