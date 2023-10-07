# set the relevant path variable
set(CMSIS_DIR       ${CMAKE_CURRENT_SOURCE_DIR}/CMSIS)
set(START_UP_DIR    ${CMSIS_DIR}/Source/gcc_startup)
set(PERIPHERALS_DIR ${CMAKE_CURRENT_SOURCE_DIR}/standard_peripheral)

# add header file search path
include_directories(
  ${CMSIS_DIR}/Include
  ${PERIPHERALS_DIR}/Include
)

# set the startup file variable
set(START_UP_ASM startup_gd32f10x_md.S)

# set the startup file C property
set_property(SOURCE ${START_UP_DIR}/${START_UP_ASM} PROPERTY LANGUAGE C)

# generate list for all files that match the query expression
file(GLOB STD_LIB_SRC $(PERIPHERALS_DIR}/Source/*c))
file(GLOB CMSIS_SRC ${CMSIS_DIR}/Source/*c)

# build the library target
add_library(firmware_library
  ${STD_LIB_SRC}
  ${CMSIS_SRC}
  ${START_UP_DIR}/${START_UP_ASM}
)

# set the name of the output library
set_target_properties(firmware_library PROPERTIES OUTPUT_NAME "lib_gd32f10x")

# set the default output path for library files
set(LIBRARY_OUTPUT_PATH $(PROJECT_SOURCE_DIR)/build/lib)
