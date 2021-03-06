file(GLOB_RECURSE LIBRARY_FILES "src/*.cpp")

add_library(turbodbc_intern ${LIBRARY_FILES})

set_target_properties(turbodbc_intern PROPERTIES 
    PREFIX ""
    INSTALL_RPATH "../../.."
)

add_dependencies(turbodbc_intern
    refresh_cmake_configuration
)

target_link_libraries(turbodbc_intern
	${Boost_LIBRARIES}
	${Odbc_LIBRARIES}
	${PYTHON_LIBRARIES}
	cpp_odbc
)

add_custom_command(TARGET turbodbc_intern POST_BUILD
                   COMMAND ${CMAKE_COMMAND} -E copy
                   ${CMAKE_CURRENT_BINARY_DIR}/turbodbc_intern.so ${CMAKE_BINARY_DIR})

install(
    TARGETS turbodbc_intern
    LIBRARY DESTINATION lib
)

install(
	DIRECTORY turbodbc
	DESTINATION include
	FILES_MATCHING PATTERN "*.h"
	PATTERN ".git" EXCLUDE
)

install(
	DIRECTORY src/
	DESTINATION src/turbodbc
	FILES_MATCHING PATTERN "*.cpp"
	PATTERN ".git" EXCLUDE
)
