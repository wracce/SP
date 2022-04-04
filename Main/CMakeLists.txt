cmake_minimum_required(VERSION 3.18)

set(PROJECT_NAME "Main")
set(CMAKE_PREFIX_PATH "C:/Qt/6.2.1/msvc2019_64") 

project(${PROJECT_NAME} VERSION 1.0 LANGUAGES CXX)

set(CMAKE_AUTOMOC ON)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Qt6 6.2.1 COMPONENTS Quick Gui REQUIRED)

qt_add_executable(${PROJECT_NAME}
    src/main.cpp
)

target_link_libraries(${PROJECT_NAME} PRIVATE Qt6::Gui Qt6::Quick)

qt6_add_qml_module(
    ${PROJECT_NAME}
    URI ${PROJECT_NAME}
    VERSION 1.0
    QML_FILES src/main.qml)

#Add dlls
if (WIN32)
    file(GLOB DLLS ${CMAKE_PREFIX_PATH}/bin/Qt6*.dll)
    foreach(DLL IN LISTS DLLS)
        add_custom_command(
                    TARGET ${PROJECT_NAME} PRE_BUILD
                    COMMAND ${CMAKE_COMMAND} -E copy_if_different ${DLL} $<TARGET_FILE_DIR:${PROJECT_NAME}>)
    endforeach()
 
    add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD COMMAND ${CMAKE_COMMAND} -E copy_directory ${CMAKE_PREFIX_PATH}\\plugins $<TARGET_FILE_DIR:Main>)

    add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD COMMAND ${CMAKE_COMMAND} -E copy_directory ${CMAKE_PREFIX_PATH}\\qml $<TARGET_FILE_DIR:${PROJECT_NAME}>)
endif ()