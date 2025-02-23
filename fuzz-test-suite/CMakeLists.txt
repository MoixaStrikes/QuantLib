# Determine the flags for fuzzing. Use OSS-Fuzz's configuration if available, otherwise fall back to defaults.
if(DEFINED ENV{LIB_FUZZING_ENGINE})
    set(FUZZING_ENGINE $ENV{LIB_FUZZING_ENGINE})
    set(FUZZING_COMPILE_FLAGS "")
    set(FUZZING_LINK_FLAGS "${FUZZING_ENGINE}")
else()
    set(FUZZING_COMPILE_FLAGS "-fsanitize=fuzzer")
    set(FUZZING_LINK_FLAGS "-fsanitize=fuzzer")
endif()

# Define the fuzz target
add_executable(DateParserFuzzer dateparserfuzzer.cpp)

# Apply the determined flags
set_target_properties(DateParserFuzzer PROPERTIES
    COMPILE_FLAGS "${FUZZING_COMPILE_FLAGS}"
    LINK_FLAGS "${FUZZING_LINK_FLAGS}"
)

# Link QuantLib and any other necessary libraries
target_link_libraries(DateParserFuzzer ql_library ${QL_THREAD_LIBRARIES})
