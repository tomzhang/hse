# empty if not HSE_UNIT_TEST_FLAG
if( ${HSE_BUILD_USER} )
include_directories( ${HSE_UT_BASE_INCLUDE_DIRS} )
include_directories( ${HSE_KVDB_UT_GS_INCLUDE_DIRS} )

include_directories(
    ../include
    ../../se_lib/
    /etc/alternatives/java_sdk/include/
    /etc/alternatives/java_sdk/include/linux/
)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DCOMPNAME=\\\"hsejni\\\"" )

set(MY_LINK_LIBS
        hse_kvdb-lib
)

set(COMPONENT_SOURCE_FILES
    hsejni.c
    hse_jni_util.c
)
  
add_library(hsejni_c SHARED
  ${COMPONENT_SOURCE_FILES}
)

add_dependencies(hsejni_c hsejni hsejni_API.h)

target_link_directories( hsejni_c PUBLIC ${MPOOL_LIB_DIR} ${BLKID_LIB_DIR} )

target_link_libraries(
    hsejni_c
        ${MY_LINK_LIBS}
        ${HSE_USER_MPOOL_LINK_LIBS}
        microhttpd
        ${LIBYAML_LIBS}
)

#
# For if/when we split this off from the main runtime package.
#
#install(TARGETS hsejni_c DESTINATION lib64 COMPONENT java)

install(TARGETS hsejni_c DESTINATION lib64 COMPONENT runtime)

endif() # ${HSE_BUILD_USER}
