project(hello VERSION 1.0.0 LANGUAGES CXX)

file(GLOB _INCLUDE "include/*.h" "include/*.hpp")
file(GLOB _SRC "src/*.[hc]pp" "src/*.cc")
file(GLOB _TEST "test/*.[hc]pp" "test/*.cc")

if(TARGET absl::strings)
    message(STATUS "Found Target: absl::strings")
else()
    message(FATAL_ERROR "Target absl::strings not found")
endif()

add_library(${PROJECT_NAME})
target_sources(${PROJECT_NAME} PRIVATE ${_INCLUDE} ${_SRC})
target_include_directories(${PROJECT_NAME} PUBLIC $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include> $<INSTALL_INTERFACE:include>)
set_target_properties(${PROJECT_NAME} PROPERTIES PUBLIC_HEADER ${_INCLUDE} VERSION ${PROJECT_VERSION} SOVERSION ${PROJECT_VERSION_MAJOR})
target_link_libraries(${PROJECT_NAME} PUBLIC absl::strings)
add_library(CMakeHello::${PROJECT_NAME} ALIAS ${PROJECT_NAME})

# Test
if(TARGET GTest::gtest_main)
message(STATUS "Found Target: GTest::gtest_main")
else()
message(FATAL_ERROR "Target GTest::gtest_main not found")
endif()
add_executable(${PROJECT_NAME}_test)
target_sources(${PROJECT_NAME}_test PRIVATE ${_TEST})
target_link_libraries(${PROJECT_NAME}_test CMakeHello::${PROJECT_NAME} GTest::gtest_main)
include(GoogleTest)
gtest_discover_tests(${PROJECT_NAME}_test)

# Install
install(TARGETS	${PROJECT_NAME}
  EXPORT HelloTargets
  PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
)
