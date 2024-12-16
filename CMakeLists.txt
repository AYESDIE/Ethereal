cmake_minimum_required(VERSION 3.15)
project(Ethereal C)

option(USE_NNUE "Enable NNUE Evaluation" OFF)
option(TUNE "Enable Tuning Build" OFF)

# NNUE-specific settings
if (USE_NNUE)
    add_definitions(-DUSE_NNUE=1)
else ()
    add_definitions(-DUSE_NNUE=0)
endif ()

# Define source files
file(GLOB SRC
    ${CMAKE_CURRENT_SOURCE_DIR}/src/*.c
    ${CMAKE_CURRENT_SOURCE_DIR}/src/pyrrhic/tbprobe.c
)

# Define libraries
set(LIBS pthread m)

# Compiler flags
set(CMAKE_C_FLAGS "-std=gnu11 -Wall -Wextra -Wshadow -O3 -DNDEBUG -march=native")

# Create the executable
add_executable(${PROJECT_NAME} ${SRC})

# Link libraries
target_link_libraries(${PROJECT_NAME} ${LIBS})

# Clean target
add_custom_target(clean-build
    COMMAND ${CMAKE_COMMAND} --build ${CMAKE_BINARY_DIR} --target clean
    COMMENT "Cleaning build directory"
)
