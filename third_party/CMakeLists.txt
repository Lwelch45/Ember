set(SEASTAR_USER_CFLAGS
  -Wno-stringop-overflow
  -Wno-array-bounds
  -Wno-stringop-truncation
  -Wno-format-overflow
  CACHE STRING "User cflags for seastar deps")
set(SEASTAR_ENABLE_TESTS OFF CACHE BOOL "disable seastar tests")
add_subdirectory(seastar)
