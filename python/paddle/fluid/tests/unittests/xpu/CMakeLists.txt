file(
  GLOB TEST_OPS
  RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}"
  "test_*.py")
string(REPLACE ".py" "" TEST_OPS "${TEST_OPS}")

if(WITH_XPU_BKCL)
  list(REMOVE_ITEM TEST_OPS "test_gen_bkcl_id_op")
endif()

file(
  GLOB DIST_TEST_OPS
  RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}"
  "test_dist_*.py")
if(WITH_XPU_BKCL)
  list(APPEND DIST_TEST_OPS test_gen_bkcl_id_op)
endif()

list(REMOVE_ITEM TEST_OPS test_concat_op_xpu)
list(REMOVE_ITEM TEST_OPS test_mean_op_xpu)

foreach(TEST_OP ${TEST_OPS})
  py_test_modules(${TEST_OP} MODULES ${TEST_OP})
endforeach()

foreach(TEST_OP ${DIST_TEST_OPS})
  py_test_modules(${TEST_OP} MODULES ${TEST_OP})
endforeach()

set_tests_properties(test_mul_op_xpu PROPERTIES TIMEOUT 120)
set_tests_properties(test_conv2d_op_xpu PROPERTIES TIMEOUT 120)
