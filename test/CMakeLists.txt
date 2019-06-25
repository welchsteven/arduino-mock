message ("building tests for Arduino Mock")

add_executable(test_all test_all.cc)

target_link_libraries(test_all
    arduino_mock
)

add_test(arduino_mock_test test_all)