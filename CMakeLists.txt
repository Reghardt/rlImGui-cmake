cmake_minimum_required(VERSION 3.14)
project(rlImGui)

# Fetch ImGui
include(FetchContent)

# Check if ImGui is already available (unlikely but good practice)
if(NOT TARGET imgui)
    FetchContent_Declare(
        imgui
        GIT_REPOSITORY https://github.com/ocornut/imgui.git
        GIT_TAG docking # Using docking branch as docking_example.cpp exists
    )
    FetchContent_MakeAvailable(imgui)
endif()

# Define the library
add_library(rlImGui STATIC
    rlImGui.cpp
    "${imgui_SOURCE_DIR}/imgui.cpp"
    "${imgui_SOURCE_DIR}/imgui_demo.cpp"
    "${imgui_SOURCE_DIR}/imgui_draw.cpp"
    "${imgui_SOURCE_DIR}/imgui_tables.cpp"
    "${imgui_SOURCE_DIR}/imgui_widgets.cpp"
)

# Include directories
target_include_directories(rlImGui PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${imgui_SOURCE_DIR}
)

# Link dependencies
# We assume 'raylib' is defined by the consumer or we could fetch it if standalone.
# For this integration, the consumer (raylib_convex) provides it.
if(TARGET raylib)
    target_link_libraries(rlImGui PUBLIC raylib)
else()
    message(WARNING "raylib target not found. Ensure raylib is available if building standalone.")
endif()

target_compile_definitions(rlImGui PUBLIC
    IMGUI_DISABLE_OBSOLETE_FUNCTIONS
    IMGUI_DISABLE_OBSOLETE_KEYIO
)
