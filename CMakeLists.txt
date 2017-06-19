cmake_minimum_required(VERSION 3.5)

project(BattleCitySDL)

file(GLOB CPP_FILES
    "BattleCitySDL/*.cpp"
)

add_executable(${PROJECT_NAME} ${CPP_FILES})

# Use C++11 standard
set_property(TARGET ${PROJECT_NAME} PROPERTY CXX_STANDARD 11)

# find library
if (WIN32)
    find_path(SDL2_PATH_PREFIX
        NAMES SDL2-2.0.5
        PATHS BattleCitySDL
    )
    set(SDL2_PATH ${SDL2_PATH_PREFIX}/SDL2-2.0.5)
    set(SDL2IMAGE_PATH ${SDL2_PATH_PREFIX}/SDL2_image-2.0.1)
    set(SDL2MIXER_PATH ${SDL2_PATH_PREFIX}/SDL2_mixer-2.0.1)

    message("SDL2_PATH: ${SDL2_PATH}")
    message("SDL2IMAGE_PATH: ${SDL2IMAGE_PATH}")
    message("SDL2MIXER_PATH: ${SDL2MIXER_PATH}")

    find_library(SDL2_LIBRARY
        NAMES SDL2
        PATHS ${SDL2_PATH}/lib/x86
    )
    find_library(SDL2MAIN_LIBRARY
        NAMES SDL2main
        PATHS ${SDL2_PATH}/lib/x86
    )
    set(SDL2_LIBRARIES ${SDL2_LIBRARY} ${SDL2MAIN_LIBRARY})
    set(SDL2_INCLUDE_DIRS ${SDL2_PATH}/include)

    find_library(SDL2IMAGE_LIBRARIES
        NAMES SDL2_image
        PATHS ${SDL2IMAGE_PATH}/lib/x86
    )
    set(SDL2IMAGE_INCLUDE_DIRS ${SDL2IMAGE_PATH}/include)

    find_library(SDL2MIXER_LIBRARIES
        NAMES SDL2_mixer
        PATHS ${SDL2MIXER_PATH}/lib/x86
    )
    set(SDL2MIXER_INCLUDE_DIRS ${SDL2MIXER_PATH}/include)

    set(OPENGL_LIBRARIES opengl32)
else()
    # find sdl2 related library
    include(FindPkgConfig)
    PKG_SEARCH_MODULE(SDL2 REQUIRED sdl2)
    PKG_SEARCH_MODULE(SDL2IMAGE REQUIRED SDL2_image>=2.0.0)
    PKG_SEARCH_MODULE(SDL2MIXER REQUIRED SDL2_mixer>=2.0.0)

    # find opengl library
    include(FindOpenGL)

    # add library search path
    link_directories(
        ${SDL2_LIBRARY_DIRS}
        ${SDL2IMAGE_LIBRARY_DIRS}
        ${SDL2MIXER_LIBRARY_DIRS}
    )
endif()

# message("SDL2_INCLUDE_DIRS: ${SDL2_INCLUDE_DIRS}")
# message("SDL2_LIBRARY_DIRS: ${SDL2_LIBRARY_DIRS}")
# message("SDL2_LIBRARIES: ${SDL2_LIBRARIES}")
#
# message("SDL2IMAGE_INCLUDE_DIRS: ${SDL2IMAGE_INCLUDE_DIRS}")
# message("SDL2IMAGE_LIBRARY_DIRS: ${SDL2IMAGE_LIBRARY_DIRS}")
# message("SDL2IMAGE_LIBRARIES: ${SDL2IMAGE_LIBRARIES}")
#
# message("SDL2MIXER_INCLUDE_DIRS: ${SDL2MIXER_INCLUDE_DIRS}")
# message("SDL2MIXER_LIBRARY_DIRS: ${SDL2MIXER_LIBRARY_DIRS}")
# message("SDL2MIXER_LIBRARIES: ${SDL2MIXER_LIBRARIES}")
#
# message("OPENGL_LIBRARIES: ${OPENGL_LIBRARIES}")

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
            $<TARGET_FILE_DIR:BattleCitySDL>/data
    COMMENT "Copying data files"
)

# copy dll files on Windows
if (WIN32)
    add_custom_command(
        TARGET ${PROJECT_NAME} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy
                ${SDL2_PATH}/lib/x86/SDL2.dll
                $<TARGET_FILE_DIR:BattleCitySDL>
        COMMENT "Copying SDL2.dll"
    )
    add_custom_command(
        TARGET ${PROJECT_NAME} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy
                ${SDL2IMAGE_PATH}/lib/x86/zlib1.dll
                $<TARGET_FILE_DIR:BattleCitySDL>
        COMMENT "Copying zlib1.dll"
    )
    add_custom_command(
        TARGET ${PROJECT_NAME} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy
                ${SDL2IMAGE_PATH}/lib/x86/SDL2_image.dll
                $<TARGET_FILE_DIR:BattleCitySDL>
        COMMENT "Copying SDL2_image.dll"
    )
    add_custom_command(
        TARGET ${PROJECT_NAME} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy
                ${SDL2MIXER_PATH}/lib/x86/SDL2_mixer.dll
                $<TARGET_FILE_DIR:BattleCitySDL>
        COMMENT "Copying SDL2_mixer.dll"
    )
    add_custom_command(
        TARGET ${PROJECT_NAME} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy
                ${SDL2MIXER_PATH}/lib/x86/libogg-0.dll
                $<TARGET_FILE_DIR:BattleCitySDL>
        COMMENT "Copying libogg-0.dll"
    )
    add_custom_command(
        TARGET ${PROJECT_NAME} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy
                ${SDL2MIXER_PATH}/lib/x86/libvorbis-0.dll
                $<TARGET_FILE_DIR:BattleCitySDL>
        COMMENT "Copying libvorbis-0.dll"
    )
    add_custom_command(
        TARGET ${PROJECT_NAME} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy
                ${SDL2MIXER_PATH}/lib/x86/libvorbisfile-3.dll
                $<TARGET_FILE_DIR:BattleCitySDL>
        COMMENT "Copying libvorbisfile-3.dll"
    )
endif()
