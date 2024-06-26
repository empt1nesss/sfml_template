cmake_minimum_required(VERSION 3.20)
project(SFML_template LANGUAGES CXX)

option(BUILD_SHARED_LIBS "Build shared libraries" OFF)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(BUILD_SOURCES
  "source/main.cpp"
)

include(FetchContent)
FetchContent_Declare(SFML
  GIT_REPOSITORY https://github.com/SFML/SFML.git
  GIT_TAG 2.6.x
)
FetchContent_MakeAvailable(SFML)


add_executable(${PROJECT_NAME} ${BUILD_SOURCES})

target_link_libraries(${PROJECT_NAME}
  PRIVATE
    sfml-graphics
)

target_include_directories(${PROJECT_NAME}
  PRIVATE
    ${CMAKE_SOURCE_DIR}/include
)


if(WIN32)
  add_custom_command(
    TARGET ${PROJECT_NAME}
    COMMENT "Copy OpenAL DLL"
    PRE_BUILD COMMAND ${CMAKE_COMMAND} -E copy ${SFML_SOURCE_DIR}/extlibs/bin/$<IF:$<EQUAL:${CMAKE_SIZEOF_VOID_P},8>,x64,x86>/openal32.dll $<TARGET_FILE_DIR:${PROJECT_NAME}>
    VERBATIM
  )
endif()

install(TARGETS ${PROJECT_NAME})
