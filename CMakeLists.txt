# forzah - ReXGlue Recompiled Project
#
# SDK boilerplate lives in generated/rexglue.cmake.

cmake_minimum_required(VERSION 3.25)
project(forzah LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include(generated/rexglue.cmake)

# Sources
set(FORZAH_SOURCES
    src/main.cpp
)
if(REXGLUE_PLATFORM_SWITCH)
    list(APPEND FORZAH_SOURCES src/switch_modulos.cpp)
endif()

if(WIN32)
    add_executable(forzah WIN32 ${FORZAH_SOURCES})
else()
    add_executable(forzah ${FORZAH_SOURCES})
endif()

# GPU_PLUGINS copia librexgpu-xenos ao lado do executável (como no nfsmw-nx).
rexglue_setup_target(forzah GPU_PLUGINS xenos)

# rex_app.h includes imgui headers, but the SDK does not export imgui's include dir
# when it is built from a source tree (REXSDK_DIR).
if(NOT REXGLUE_PLATFORM_SWITCH AND TARGET imgui)
    target_include_directories(forzah PRIVATE $<TARGET_PROPERTY:imgui,INTERFACE_INCLUDE_DIRECTORIES>)
    target_include_directories(forzah PRIVATE "${REXSDK_DIR}/thirdparty/imgui")
endif()

# Nintendo Switch: forzah.nro ao lado do ELF. No cartão SD: sdmc:/switch/forzah/forzah.nro
# com a pasta game_root/ (os arquivos extraídos do seu Forza Horizon) ao lado.
if(REXGLUE_PLATFORM_SWITCH)
    option(FORZAH_LTO "LTO no código traduzido e no app (Switch); pesado em RAM" OFF)
    if(FORZAH_LTO)
        foreach(objetivo forzah_recomp forzah)
            if(TARGET ${objetivo})
                target_compile_options(${objetivo} PRIVATE -flto -fno-fat-lto-objects)
            endif()
        endforeach()
        target_link_options(forzah PRIVATE -flto=2 -flto-partition=balanced)
    endif()
    set(FORZAH_ICONO "${CMAKE_CURRENT_SOURCE_DIR}/tools/switch/icono/forzah_icono.jpg")
    set(FORZAH_ICONO_ARG)
    if(EXISTS "${FORZAH_ICONO}")
        set(FORZAH_ICONO_ARG ICON "${FORZAH_ICONO}")
    endif()
    if(FORZAH_SWITCH_DLL_MODULES)
        target_link_libraries(forzah PRIVATE ${FORZAH_SWITCH_DLL_MODULES})
    endif()
    rexglue_switch_add_nro(forzah NAME "Forza Horizon" AUTHOR "forza-nx" VERSION "0.1.0" ${FORZAH_ICONO_ARG})
endif()
