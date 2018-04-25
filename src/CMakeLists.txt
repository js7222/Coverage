set(PROJECT_TEST_NAME "main")
add_executable(${PROJECT_TEST_NAME} main.cc)

include_directories(${Boost_INCLUDE_DIR})
add_test (NAME UnitTests COMMAND ${PROJECT_TEST_NAME})

#add_custom_target(ctest COMMAND ${CMAKE_CTEST_COMMAND})
if (ENABLE_COVERAGE MATCHES ON)
  SETUP_TARGET_FOR_COVERAGE(NAME coverageTarget EXECUTABLE main "testResults")
endif()

