

include_directories(
    ../common
    ../common/qoptargparse
    ../common/database
    ../common/qsimplecfg
    ../common/oscpp
    ../common/database
    ../common/qsqlthrow
    ../../extern/xxHash
    )

add_executable(${PROJECT_NAME} ../../html-export/dist/htmlexportres.qrc
    shournal.cpp # main
    argcontrol_dbdelete
    argcontrol_dbquery
    command_printer
    command_printer_html
    command_printer_human
    command_printer_json
    cmd_stats
    )

# To keep dependencies low, only generate the main.js
# when developing the html export.
# To do so, set HTML_EXPORT_DEV to ON:
# cmake -DHTML_EXPORT_DEV:BOOL=ON
# npm and webpack must already be installed
if (${HTML_EXPORT_DEV})
    add_custom_target(
        build_htmlplot_npm
        WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}/html-export
        COMMAND npm install --production
        COMMAND npm run build
        )
    add_dependencies(${PROJECT_NAME} build_htmlplot_npm)
endif()



target_link_libraries(${PROJECT_NAME}
    lib_shournal_common
    )

install(
    TARGETS ${PROJECT_NAME}
    RUNTIME DESTINATION bin
    PERMISSIONS
                OWNER_READ OWNER_WRITE OWNER_EXECUTE
                GROUP_READ GROUP_EXECUTE
                WORLD_READ WORLD_EXECUTE
)
