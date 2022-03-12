# ---- Options ----
option(ENABLE_TEST_COVERAGE "Enable test coverage" OFF)

cmake_minimum_required(VERSION 3.14)

project(Csd VERSION 1.0)
include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup()

# ---- Create test ----

add_executable(${PROJECT_NAME}Tests test_csd.cpp csd.cpp)
set_target_properties(${PROJECT_NAME}Tests PROPERTIES CXX_STANDARD 14)
target_link_libraries(${PROJECT_NAME}Tests ${CONAN_LIBS})

# ---- Add ${PROJECT_NAME}Tests ----

enable_testing()

# Note: doctest and similar testing frameworks can automatically configure CMake
# tests. For other testing frameworks add the tests target instead:
# add_test(NAME ecgenTests COMMAND ${PROJECT_NAME}Tests)

string(TOLOWER ${PROJECT_NAME} PROJECT_FILE_NAME)
add_test(NAME ${PROJECT_FILE_NAME}Tests COMMAND ${PROJECT_NAME}Tests)

# ---- code coverage ----

if(ENABLE_TEST_COVERAGE)
  target_compile_options(${PROJECT_NAME}Tests PUBLIC -O0 -g -fprofile-arcs
                                                -ftest-coverage)
  target_link_options(${PROJECT_NAME}Tests PUBLIC -fprofile-arcs -ftest-coverage)
endif()
