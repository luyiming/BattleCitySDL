cmake_minimum_required(VERSION 3.5)

project(BattleCitySDL)

file(GLOB CPP_FILES
    "BattleCitySDL/*.cpp"
)

add_executable(${PROJECT_NAME} ${CPP_FILES})

# Use C++11 standard
set_property(TARGET ${PROJECT_NAME} PROPERTY CXX_STANDARD 11)

include(FindPkgConfig)

PKG_SEARCH_MODULE(SDL2 REQUIRED sdl2)
PKG_SEARCH_MODULE(SDL2IMAGE REQUIRED SDL2_image>=2.0.0)
PKG_SEARCH_MODULE(SDL2MIXER REQUIRED SDL2_mixer>=2.0.0)

message("SDL2_LIBRARIES: ${SDL2_LIBRARIES}")
message("SDL2_LIBRARY_DIRS: ${SDL2_LIBRARY_DIRS}")
message("SDL2_INCLUDE_DIRS: ${SDL2_INCLUDE_DIRS}")
message("SDL2_VERSION: ${SDL2_VERSION}")

include(FindOpenGL)

link_directories(
    ${SDL2_LIBRARY_DIRS}
    ${SDL2IMAGE_LIBRARY_DIRS}
    ${SDL2MIXER_LIBRARY_DIRS}
)

target_include_directories(${PROJECT_NAME}
    PUBLIC ${SDL2_INCLUDE_DIRS}
    PUBLIC ${SDL2IMAGE_INCLUDE_DIRS}
    PUBLIC ${SDL2MIXER_INCLUDE_DIRS}
    PUBLIC ${OPENGL_INCLUDE_DIR}
)

target_link_libraries(${PROJECT_NAME}
    ${SDL2_LIBRARIES}
    ${SDL2IMAGE_LIBRARIES}
    ${SDL2MIXER_LIBRARIES}
    ${OPENGL_LIBRARIES}
)

# copy data files
add_custom_command(
    TARGET ${PROJECT_NAME} POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
            ${CMAKE_SOURCE_DIR}/data
            ${CMAKE_CURRENT_BINARY_DIR}/data
    COMMENT "Copying 'data/'")
