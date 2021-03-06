
file(GLOB_RECURSE SOURCES "src/*.cpp" "src/*.h" "src/*.hpp")

if (${WIN32})
  set (BOOST_LIBS_LIST )

  elseif (${APPLE})
   set (BOOST_LIBS_LIST
         Boost::filesystem 
         Boost::program_options 
         Boost::system
      )
else()
  set (BOOST_LIBS_LIST
         boost_filesystem 
         boost_program_options 
         boost_system
      )
endif (${WIN32})
  
add_executable(sc-builder ${SOURCES})
target_include_directories(sc-builder 
  PRIVATE ${SC_MEMORY_SRC}
  PRIVATE ${GLIB2_INCLUDE_DIRS}
  )
target_link_libraries(sc-builder sc-core sc-memory ${BOOST_LIBS_LIST})

add_dependencies(sc-builder sc-code-generator sc-kpm sc-memory)

sc_codegen(sc-builder ${CMAKE_CURRENT_SOURCE_DIR}/src)

if (${SC_BUILD_TESTS})
  add_subdirectory(tests)
endif()