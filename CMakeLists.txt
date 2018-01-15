cmake_minimum_required(VERSION 2.8)
set(CMAKE_VERBOSE_MAKEFILE 1)
 
project(hiberlite)
set(CMAKE_CXX_FLAGS "-std=c++11 -Wall -Wextra -pedantic -Wcast-align -Wcast-qual -Wconversion -Wdisabled-optimization -Wendif-labels -Wfloat-equal -Winit-self -Winline -Wlogical-op -Wmissing-include-dirs -Wnon-virtual-dtor -Wold-style-cast -Woverloaded-virtual -Wpacked -Wpointer-arith -Wredundant-decls -Wshadow -Wsign-promo -Wswitch-default -Wswitch-enum -Wunsafe-loop-optimizations -Wvariadic-macros -Wwrite-strings -fPIC -includesstream")
set(CMAKE_CXX_FLAGS_DEBUG "-g3 -O0 -pg")
set(CMAKE_CXX_FLAGS_RELEASE "-O2 -s -DNDEBUG -march=native")
 
include_directories(./include/ ./sqlite-amalgamation)
file(GLOB_RECURSE SRCS ./src/*.cpp  sqlite-amalgamation/sqlite3.c )
message( ${SRCS} )

# add_executable(hiberlite ${SRCS} sample.cpp)
project(sample)
add_library(hiberlite STATIC ${SRCS})

add_executable(sample sample.cpp)
set(CMAKE_FIND_LIBRARY_PREFIXES "lib")
set(CMAKE_FIND_LIBRARY_SUFFIXES  ".so" ".dylib")
find_library(LIBPTHREAD_LIBRARY NAMES pthread)
message(${LIBPTHREAD_LIBRARY})
find_library(LIBDL_LIBRARY NAMES dl)
message(${LIBDL_LIBRARY})
target_link_libraries(sample hiberlite ${LIBPTHREAD_LIBRARY} ${LIBDL_LIBRARY})
 

project(tests)
add_executable(tests tests.cpp)
target_link_libraries(tests hiberlite ${LIBPTHREAD_LIBRARY} ${LIBDL_LIBRARY})