cmake_minimum_required(VERSION 3.12)

project(cpp-web
    VERSION 0.0.1
    LANGUAGES C CXX
    DESCRIPTION "A simple web server for C++")

if (WIN32) #gross
    set(BOOST_ROOT "C:\\Program Files\\boost\\boost_1_75_0")
    set(BOOST_LIBS "C:\\Program Files\\boost\\boost_1_75_0\\stage\\lib")
endif()

find_package(Boost 1.68.0 REQUIRED)
find_package( Threads ) # Required to find pthreads

add_executable(${PROJECT_NAME}
    src/main.cpp
    src/connection_manager.cpp
    src/connection.cpp
    src/mime_types.cpp
    src/reply.cpp
    src/request_handler.cpp
    src/request_parser.cpp
    src/server.cpp
)

target_include_directories(${PROJECT_NAME} PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
    ${Boost_INCLUDE_DIRS} #for windows support
)

target_link_libraries(${PROJECT_NAME} 
    ${CMAKE_THREAD_LIBS_INIT}
    ${BOOST_LIBS}/libboost_date_time-vc142-mt-gd-x64-1_75.lib # I hate it Windows!
    ${BOOST_LIBS}/libboost_regex-vc142-mt-gd-x64-1_75.lib
)