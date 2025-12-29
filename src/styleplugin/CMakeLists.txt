cmake_minimum_required(VERSION 3.16)

project(phantomstyleplugin LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

find_package(Qt6 REQUIRED COMPONENTS Core Gui Widgets)

add_subdirectory(../phantom phantom_build)

qt_add_plugin(phantomstyleplugin
    phantomstyleplugin.cpp
    phantomstyleplugin.h
    phantomstyleplugin.json
)

target_link_libraries(phantomstyleplugin
    PRIVATE
    Qt6::Core
    Qt6::Gui
    Qt6::Widgets
    PhantomStyle
)

target_compile_definitions(phantomstyleplugin PRIVATE QT_DEPRECATED_WARNINGS)

if(MSVC)
    target_compile_options(phantomstyleplugin PRIVATE /GR- /EHs-c-)
    target_compile_options(phantomstyleplugin PRIVATE
        /Wall
        /wd4365 /wd4820 /wd4623 /wd4625 /wd4626 /wd5026 /wd5027 /wd4619 /wd4548 /wd4774 /wd4946 /wd4371
        /wd4710 /wd4711 /wd4068 /w14062 /wd5045
    )
    target_compile_options(phantomstyleplugin PRIVATE $<$<CONFIG:Release>:/wd4702>)

elseif(CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    target_compile_options(phantomstyleplugin PRIVATE 
        -fno-rtti -fno-exceptions -Wall -Wno-format-security -Wno-unused-function -Wno-unknown-pragmas
    )

elseif(CMAKE_CXX_COMPILER_ID MATCHES "GNU")
    target_compile_options(phantomstyleplugin PRIVATE 
        -fno-rtti 
        -fno-exceptions
        -Wno-unused-function 
        -Wno-unknown-pragmas
    )
endif()
