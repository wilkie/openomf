set(LIBSHADOWDIVE_LIBRARY_ONLY 1)

find_program(GIT_COMMAND git)

if( NOT EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/libShadowDive/src" OR NOT EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/dumb/dumb")
    if(GIT_COMMAND)
        execute_process(
            COMMAND ${GIT_COMMAND} submodule update --init --recursive
            WORKING_DIRECTORY ${PROJECT_SOURCE_DIR})
    else(GIT_COMMAND)
        MESSAGE(FATAL_ERROR "Could not locate the git command, Please manually run 'git submodule update --init --recursive'")
    endif(GIT_COMMAND)
endif()

if(GIT_COMMAND)
    add_custom_target(update-deps
        COMMAND ${GIT_COMMAND} submodule update --init --recursive
        WORKING_DIRECTORY ${PROJECT_SOURCE_DIR})
endif(GIT_COMMAND)
    
add_subdirectory(libShadowDive)
add_subdirectory(dumb/dumb/cmake)

# Includes
set(DUMB_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/dumb/dumb/include" PARENT_SCOPE)
set(SHADOWDIVE_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/libShadowDive/include" PARENT_SCOPE)
