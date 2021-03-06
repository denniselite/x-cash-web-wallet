

macro(add_om_test _TEST_NAME)

    add_executable(${_TEST_NAME}_tests
            ${_TEST_NAME}_tests.cpp
            JsonTx.cpp
            #${MONERO_DIR}/tests/core_tests/chaingen.cpp
            #${MONERO_DIR}/tests/core_tests/chaingen001.cpp
            )

    target_link_libraries(${_TEST_NAME}_tests
            gtest gtest_main
            gmock gmock_main
            ${LIBRARIES})

    target_include_monero_directories(${_TEST_NAME}_tests)

    target_include_directories(${_TEST_NAME}_tests
     PRIVATE ../src/xmregcore)

    add_test(NAME ${_TEST_NAME}_tests COMMAND ${_TEST_NAME}_tests)

endmacro()

resource_dir("res")

add_om_test(mysql)
add_om_test(microcore)
add_om_test(bcstatus)
add_om_test(txsearch)

SETUP_TARGET_FOR_COVERAGE(
        NAME mysql_cov
        EXECUTABLE mysql_tests)

SETUP_TARGET_FOR_COVERAGE(
        NAME microcore_cov
        EXECUTABLE microcore_tests)

SETUP_TARGET_FOR_COVERAGE(
        NAME bcstatus_cov
        EXECUTABLE bcstatus_tests)

SETUP_TARGET_FOR_COVERAGE(
        NAME txsearch_cov
        EXECUTABLE txsearch_tests)

add_custom_target(cov
    COMMAND ;
    DEPENDS mysql_cov microcore_cov bcstatus_cov txsearch_cov
    COMMENT "Runs all coverage tests"
)
