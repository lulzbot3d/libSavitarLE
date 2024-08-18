find_package(GTest REQUIRED)
include(GoogleTest)

set(savitarle_TEST
        ThreeMFParserTest
        MeshDataTest
        NamespaceTest
        )

foreach (test ${savitarle_TEST})
    add_executable(${test} main.cpp ${test}.cpp)
    target_link_libraries(${test} SavitarLE GTest::gtest GTest::gmock)
    add_test(${test} ${test})
endforeach()