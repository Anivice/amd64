cmake_minimum_required(VERSION 3.30)
project(amd64 ASM)

find_program(NASM_EXECUTABLE nasm REQUIRED)

function(add_nasm_target OUTPUT_FILENAME INPUT_FILENAME)
    set(TARGET_NAME "__nasm_target__${OUTPUT_FILENAME}__")
    message(STATUS "Pending NASM target ${TARGET_NAME} from source file ${INPUT_FILENAME} to binary file ${OUTPUT_FILENAME}...")
    # Generate binary file from assembly source
    add_custom_command(
            OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/${OUTPUT_FILENAME}
            COMMAND ${NASM_EXECUTABLE} -O0 -X gnu -f bin ${CMAKE_CURRENT_SOURCE_DIR}/${INPUT_FILENAME} -o ${CMAKE_CURRENT_BINARY_DIR}/${OUTPUT_FILENAME}
            DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/${INPUT_FILENAME}
            COMMENT "Assembling ${INPUT_FILENAME} to ${OUTPUT_FILENAME}..."
    )

    # Create a target to manage dependencies and build
    add_custom_target(${TARGET_NAME} ALL
            DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/${OUTPUT_FILENAME}
            COMMENT "Building binary target ${TARGET_NAME} for file ${OUTPUT_FILENAME}..."
    )
endfunction()

add_nasm_target(simple_bin simple_test.asm)
