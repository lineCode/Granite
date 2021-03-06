add_granite_library(application-input input.hpp input.cpp)
target_include_directories(application-input PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
target_link_libraries(application-input vulkan-backend math ui-renderer event)

if (${CMAKE_SYSTEM} MATCHES "Linux")
    include(FindPkgConfig)
    pkg_check_modules(HOTPLUG_UDEV libudev)
    if (HOTPLUG_UDEV_FOUND)
        add_granite_library(input-linux input_linux.cpp input_linux.hpp)
        target_include_directories(input-linux PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
        target_link_libraries(input-linux util application-input ${HOTPLUG_UDEV_LIBRARIES})
        target_compile_options(input-linux PUBLIC ${HOTPLUG_UDEV_CFLAGS_OTHER})
        target_include_directories(input-linux PUBLIC ${HOTPLUG_UDEV_INCLUDE_DIRS})
    endif()
elseif (WIN32)
    add_granite_library(xinput-windows xinput_windows.cpp xinput_windows.hpp)
    target_include_directories(xinput-windows PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
    target_link_libraries(xinput-windows util application-input Xinput)
endif()
