# Copyright (C) 2020 Intel Corporation
# SPDX-License-Identifier: Apache-2.0
#

# [cmake:plugin]
set(TARGET_NAME "templatePlugin")

if(ENABLE_LTO)
    ie_enable_lto()
endif()

file(GLOB SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp)
file(GLOB_RECURSE HEADERS ${CMAKE_CURRENT_SOURCE_DIR}/*.hpp)

# adds a shared library with plugin
ie_add_plugin(NAME ${TARGET_NAME}
              DEVICE_NAME "TEMPLATE"
              SOURCES ${SOURCES} ${HEADERS}
              SKIP_INSTALL # ATTENTION: uncomment to install component
              VERSION_DEFINES_FOR template_plugin.cpp)

target_include_directories(${TARGET_NAME} PRIVATE
    "${CMAKE_CURRENT_SOURCE_DIR}")

target_include_directories(${TARGET_NAME} PRIVATE
    "${IE_MAIN_TEMPLATE_PLUGIN_SOURCE_DIR}/include")

# link common Inference Engine libraries
target_link_libraries(${TARGET_NAME} PRIVATE
    IE::inference_engine
    IE::inference_engine_transformations
    ${NGRAPH_LIBRARIES})

# Link inference backend library to plugin. Here we use ngraph interpreter_backend as example
target_link_libraries(${TARGET_NAME} PRIVATE
    IE::ngraph_backend
    IE::interpreter_backend)

# ATTENTION: uncomment to register a plugin in the plugins.xml file
# ie_register_plugins(MAIN_TARGET ${TARGET_NAME}
#                     POSSIBLE_PLUGINS ${TARGET_NAME})
# [cmake:plugin]

# ATTENTION: uncomment to install component
# install

# set(component_name template)
# ie_cpack_add_component(${component_name} REQUIRED)

# install(TARGETS ${TARGET_NAME}
#         RUNTIME DESTINATION ${IE_CPACK_RUNTIME_PATH}
#         ARCHIVE DESTINATION ${IE_CPACK_ARCHIVE_PATH}
#         LIBRARY DESTINATION ${IE_CPACK_LIBRARY_PATH}
#         COMPONENT ${component_name})
