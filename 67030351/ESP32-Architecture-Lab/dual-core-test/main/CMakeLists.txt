
    idf_component_register(SRCS "dual_core_test.c"
                    INCLUDE_DIRS ".")

target_link_libraries(${COMPONENT_LIB} m)
