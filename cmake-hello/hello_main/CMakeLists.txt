project(hello_main VERSION 1.0.0 LANGUAGES CXX)

file(GLOB _SRC "src/*.[hc]pp" "src/*.cc")

if(TARGET hello)
    message(STATUS "Found Target: hello")
else()
    message(FATAL_ERROR "Target hello not found")
endif()

add_executable(${PROJECT_NAME})
target_sources(${PROJECT_NAME} PRIVATE ${_SRC})
target_include_directories(${PROJECT_NAME} PRIVATE ${PROJECT_SOURCE_DIR}/src)
set_target_properties(${PROJECT_NAME} PROPERTIES VERSION ${PROJECT_VERSION})
target_link_libraries(${PROJECT_NAME} PRIVATE CMakeHello::hello)
add_executable(CMakeHello::${PROJECT_NAME} ALIAS ${PROJECT_NAME})

# Install
install(TARGETS	${PROJECT_NAME}
  EXPORT HelloTargets
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)
