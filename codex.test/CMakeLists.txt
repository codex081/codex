project( codex.test )

include_directories( ${CMAKE_CURRENT_SOURCE_DIR}/../codex/include )
include_directories( ${CMAKE_CURRENT_SOURCE_DIR}/../googletest/googletest/include )

link_directories( ${codex.workspace_BINARY_DIR}/codex )

if( MSVC )

	set(BOOST_ROOT "D:/tk/work/local/boost_1_61_0")
	set(Boost_USE_STATIC_LIBS   	ON)
	set(Boost_USE_MULTITHREADED 	ON)
	set(Boost_USE_STATIC_RUNTIME  ON)

	add_definitions (/D _CRT_SECURE_NO_WARNINGS)
else()
	add_definitions( -Wall -std=c++11 -o2 -g -ggdb )
	if( APPLE )

	else()

	endif()
endif()

find_package (Boost 1.55.0 REQUIRED COMPONENTS system date_time regex )

if (Boost_FOUND)
  include_directories (SYSTEM ${Boost_INCLUDE_DIR})
endif()

if ( MSVC )
  set( PLATFORM_DEPEND_HEADERS

    )
  set( PLATFORM_DEPEND_SRCS

    )
else()
  if ( APPLE )
    set( PLATFORM_DEPEND_HEADERS

      )
    set( PLATFORM_DEPEND_SRCS

      )
  else()
    set( PLATFORM_DEPEND_HEADERS
      )
    set( PLATFORM_DEPEND_SRCS
      test_epoll.cpp
      )
  endif()
  set( PLATFORM_DEPEND_HEADERS
    ${PLATFORM_DEPEND_HEADERS}
    )
  set( PLATFORM_DEPEND_SRCS
    ${PLATFORM_DEPEND_SRCS}
    )
endif()
add_executable( codex.test
	test_codex.cpp
	test_diag.cpp
  test_chrono.cpp
  test_buffer.cpp
  test_handler.cpp
  test_base64.cpp
  test_string.cpp
  test_loop.cpp
  test_udp.cpp
  test_variadic.cpp
  test_object_pool.cpp
  test_address.cpp
  ${PLATFORM_DEPEND_SRCS}
)

target_link_libraries( codex.test
 codex
 gtest
 gtest_main
 ${Boost_LIBRARIES}
)
