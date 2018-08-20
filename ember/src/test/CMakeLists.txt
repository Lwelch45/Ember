add_executable(test_server test.cc)
target_link_libraries(test_server seastar)
target_include_directories(test_server
  PUBLIC ${PROJECT_SOURCE_DIR}/ember/include
)

if(CMAKE_BUILD_TYPE MATCHES Debug)
  target_compile_definitions(test_server PUBLIC -DSMF_DEBUG)
  target_compile_options(test_server PRIVATE
    -fdiagnostics-color=auto
    -Wall
    -Werror
    -Wextra
    -Wformat
    -Wmissing-braces
    -Wparentheses
    -Wpointer-arith
    -Wformat-security
    -Wunused
    -Wno-unused-parameter
    -Wcast-align
    -Wno-missing-field-initializers
    -Wdelete-non-virtual-dtor
    -Wno-ignored-qualifiers)
endif()

install(TARGETS test_server DESTINATION bin)