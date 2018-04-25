
# Download and unpack gtest at configure time
configure_file(gtest-CMakeLists.txt gtest-download/CMakeLists.txt)
execute_process(COMMAND ${CMAKE_COMMAND} -G "${CMAKE_GENERATOR}" .
  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/gtest-download )
execute_process(COMMAND ${CMAKE_COMMAND} --build .
  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/gtest-download )
 
# Add gtest directly to our build
add_subdirectory(${CMAKE_CURRENT_BINARY_DIR}/gtest-src
                 ${CMAKE_CURRENT_BINARY_DIR}/gtest-build
                 EXCLUDE_FROM_ALL )
 
# Add the gtest include directory, since gtest
# doesn't add that dependency to its gtest target
target_include_directories(gtest INTERFACE
  "${CMAKE_BINARY_DIR}/test/gtest-src/include" )

add_executable(mytest mytest.cc)
target_link_libraries(mytest main gtest_main)
target_include_directories(mytest PRIVATE ${CMAKE_SOURCE_DIR}/src)
add_test(NAME mytest COMMAND mytest)

add_custom_target(ctest COMMAND ${CMAKE_CTEST_COMMAND} DEPENDS mytest)
if (ENABLE_COVERAGE MATCHES ON)
  setup_target_for_coverage(NAME coverageTarget EXECUTABLE ctest "testResults" DEPENDENCIES ctest)
endif()
