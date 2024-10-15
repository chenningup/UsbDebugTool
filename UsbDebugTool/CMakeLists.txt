cmake_minimum_required(VERSION 3.16)

project(UsbDebugTool VERSION 0.1 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Qt6 6.4 REQUIRED COMPONENTS Quick)

qt_standard_project_setup()


add_subdirectory(../XuCommunications XuCommunications)
include_directories("${PROJECT_SOURCE_DIR}/../3rd/")
if(WIN32)
    link_directories("${PROJECT_SOURCE_DIR}/../Libs/Windows/")
elseif(UNIX)
    link_directories("${PROJECT_SOURCE_DIR}/../Libs/Linux/")
endif()
qt_add_executable(appUsbDebugTool
    main.cpp
)

qt_add_qml_module(appUsbDebugTool
    URI UsbDebugTool
    VERSION 1.0
    QML_FILES
    Main.qml
)

# Qt for iOS sets MACOSX_BUNDLE_GUI_IDENTIFIER automatically since Qt 6.1.
# If you are developing for iOS or macOS you should consider setting an
# explicit, fixed bundle identifier manually though.
set_target_properties(appUsbDebugTool PROPERTIES
#    MACOSX_BUNDLE_GUI_IDENTIFIER com.example.appUsbDebugTool
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    MACOSX_BUNDLE TRUE
    WIN32_EXECUTABLE TRUE
)

if(WIN32)
    target_link_libraries(appUsbDebugTool
        PRIVATE Qt6::Quick XuCommunications libusb-1.0
    )
elseif(UNIX)
    target_link_libraries(appUsbDebugTool
        PRIVATE Qt6::Quick XuCommunications usb-1.0
    )
endif()


include(GNUInstallDirs)
install(TARGETS appUsbDebugTool
    BUNDLE DESTINATION .
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)
