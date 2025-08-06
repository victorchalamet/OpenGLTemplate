cmake_minimum_required(VERSION 3.10)
project(ProjectName)

# Compile en C++17
set(CMAKE_CXX_STANDARD 17)

# Ajoute les sources
file(GLOB_RECURSE SRC_FILES
    src/*.cpp
    src/*.c
)

add_executable(ProjectName ${SRC_FILES})

# Inclure les headers
target_include_directories(ProjectName
    PRIVATE
        ${CMAKE_SOURCE_DIR}/include
)

# Lier les bibliothèques statiques/dynamiques (dans lib/)
target_link_libraries(ProjectName
    PRIVATE
        ${CMAKE_SOURCE_DIR}/lib/libglfw3dll.a
        ${CMAKE_SOURCE_DIR}/lib/libassimp.dll.a
        opengl32
)

add_custom_command(TARGET ProjectName POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
        ${CMAKE_SOURCE_DIR}/glfw3.dll
        $<TARGET_FILE_DIR:ProjectName>/glfw3.dll
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
        ${CMAKE_SOURCE_DIR}/libassimp.dll
        $<TARGET_FILE_DIR:ProjectName>/libassimp.dll
    COMMAND ${CMAKE_COMMAND} -E copy_directory
        ${CMAKE_SOURCE_DIR}/resources
        $<TARGET_FILE_DIR:ProjectName>/resources
    COMMAND ${CMAKE_COMMAND} -E copy_directory
        ${CMAKE_SOURCE_DIR}/Shader
        $<TARGET_FILE_DIR:ProjectName>/Shader
)