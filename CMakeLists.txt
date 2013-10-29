project(LunaSysMgr)

cmake_minimum_required(VERSION 2.8.8)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC TRUE)

include(webOS/webOS)
webos_modules_init(1 0 0 QUALIFIER RC3)
webos_component(3 0 0)

webos_add_compiler_flags(ALL -fno-rtti -fno-exceptions -fvisibility=hidden -fvisibility-inlines-hidden -Wall -fpermissive)
webos_add_compiler_flags(ALL -Wno-unused-parameter -Wno-unused-variable -Wno-reorder -Wno-missing-field-initializers -Wno-extra)
webos_add_compiler_flags(ALL -DFIX_FOR_QT)
webos_add_compiler_flags(ALL -DHAS_NYX)
webos_add_compiler_flags(ALL -DQT_NO_KEYWORDS)
webos_add_compiler_flags(ALL -DHAS_LUNA_PREF)
webos_add_compiler_flags(ALL -DHAS_DISPLAY_TIMEOUT)

find_package(Qt5Core REQUIRED)
if(NOT Qt5Core_FOUND)
    message(FATAL_ERROR "Qt5Core module is required!")
endif()

find_package(Qt5Gui REQUIRED)
if(NOT Qt5Gui_FOUND)
    message(FATAL_ERROR "Qt5Gui module is required!")
endif()

find_package(Qt5Widgets REQUIRED)
if(NOT Qt5Widgets_FOUND)
    message(FATAL_ERROR "Qt5Widgets module is required!")
endif()

find_package(PkgConfig "0.22" REQUIRED)

pkg_check_modules(GLIB2 glib-2.0 REQUIRED)
pkg_check_modules(GTHREAD2 gthread-2.0 REQUIRED)
pkg_check_modules(LS2 luna-service2 REQUIRED)
pkg_check_modules(LUNA_SYSMGR_COMMON LunaSysMgrCommon REQUIRED)
pkg_check_modules(LUNA_SYSMGR_IPC LunaSysMgrIpc REQUIRED)
pkg_check_modules(LUNA_SYSMGR_IPC_MESSAGES LunaSysMgrIpcMessages REQUIRED)
pkg_check_modules(LUNA_PREFS luna-prefs REQUIRED)
pkg_check_modules(NYX nyx REQUIRED)
pkg_check_modules(PBNJSON_CPP pbnjson_cpp REQUIRED)
pkg_check_modules(PBNJSON_C pbnjson_c REQUIRED)
pkg_check_modules(CJSON cjson REQUIRED)
pkg_check_modules(SQLITE3 sqlite3 REQUIRED)
pkg_check_modules(OPENSSL openssl REQUIRED)
pkg_check_modules(SERVICEINSTALLER serviceinstaller REQUIRED)
pkg_check_modules(PMLOGLIB PmLogLib REQUIRED)
pkg_check_modules(ROLEGEN rolegen REQUIRED)

include_directories(
    ${GLIB2_INCLUDE_DIRS}
    ${GTHREAD2_INCLUDE_DIRS}
    ${LS2_INCLUDE_DIRS}
    ${LUNA_SYSMGR_COMMON_INCLUDE_DIRS}
    ${LUNA_SYSMGR_IPC_INCLUDE_DIRS}
    ${LUNA_SYSMGR_IPC_MESSAGES_INCLUDE_DIRS}
    ${LUNA_PREFS_INCLUDE_DIRS}
    ${NYX_INCLUDE_DIRS}
    ${PBNJSON_CPP_INCLUDE_DIRS}
    ${SQLITE3_INCLUDE_DIRS}
    ${CJSON_INCLUDE_DIRS}
    ${OPENSSL_INCLUDE_DIRS}
    ${SERVICEINSTALLER_INCLUDE_DIRS}
    ${PMLOGLIB_INCLUDE_DIRS}
    ${ROLEGEN_INCLUDE_DIRS}
    ${CMAKE_SOURCE_DIR}/Src/base
    ${CMAKE_SOURCE_DIR}/Src/base/application
    ${CMAKE_SOURCE_DIR}/Src/base/settings
    ${CMAKE_SOURCE_DIR}/Src/core
    ${CMAKE_SOURCE_DIR}/Src/remote)

set(HEADERS
    Src/base/AmbientLightSensor.h
    Src/base/LsmUtils.h
    Src/base/settings/AnimationSettings.h
    Src/base/settings/DeviceInfo.h
    Src/base/DisplayStates.h
    Src/base/HapticsController.h
    Src/base/CpuAffinity.h
    Src/base/AmbientLightSensor.cpp
    Src/base/SuspendBlocker.h
    Src/base/CircularBuffer.h
    Src/base/MemoryMonitor.h
    Src/base/EASPolicyManager.h
    Src/base/SharedGlobalProperties.h
    Src/base/Security.h
    Src/base/SystemService.h
    Src/base/BootManager.h
    Src/base/DisplayManager.h
    Src/base/EventReporter.h
    Src/base/BackupManager.h
    Src/base/InputEventMonitor.h
    Src/base/application/ServiceDescription.h
    Src/base/application/ApplicationStatus.h
    Src/base/application/ApplicationInstaller.h
    Src/base/application/PackageDescription.h
    Src/base/application/CmdResourceHandlers.h
    Src/base/application/ApplicationManager.h
    Src/base/application/MimeSystem.h
    Src/base/application/LaunchPoint.h
    Src/base/application/ApplicationDescription.h
    Src/base/application/ApplicationInstallerErrors.h
    Src/base/application/LaunchPoint.cpp
    Src/core/PtrArray.h
    Src/core/AnimationEquations.h
    Src/core/GraphicsDefs.h
    Src/remote/WebAppMgrProxy.h
    Src/remote/NativeAppManager.h)

set(SOURCES
    Src/base/Security.cpp
    Src/base/DisplayStates.cpp
    Src/base/HapticsController.cpp
    Src/base/settings/Settings.cpp
    Src/base/settings/DeviceInfo.cpp
    Src/base/settings/AnimationSettings.cpp
    Src/base/EventReporter.cpp
    Src/base/MemoryMonitor.cpp
    Src/base/DisplayManager.cpp
    Src/base/CpuAffinity.cpp
    Src/base/LsmUtils.cpp
    Src/base/JSONUtils.cpp
    Src/base/AmbientLightSensor.cpp
    Src/base/BackupManager.cpp
    Src/base/EASPolicyManager.cpp
    Src/base/SuspendBlocker.cpp
    Src/base/SystemService.cpp
    Src/base/BootManager.cpp
    Src/base/Logging.cpp
    Src/base/InputEventMonitor.cpp
    Src/base/application/ApplicationDescription.cpp
    Src/base/application/MimeSystem.cpp
    Src/base/application/PackageDescription.cpp
    Src/base/application/ApplicationInstaller.cpp
    Src/base/application/CmdResourceHandlers.cpp
    Src/base/application/ServiceDescription.cpp
    Src/base/application/ApplicationManager.cpp
    Src/base/application/ApplicationStatus.cpp
    Src/base/application/LaunchPoint.cpp
    Src/base/application/ApplicationManagerService.cpp
    Src/core/MallocHooks.cpp
    Src/core/KeywordMap.cpp
    Src/remote/WebAppMgrProxy.cpp
    Src/remote/NativeAppManager.cpp
    Src/Main.cpp)

add_executable(LunaSysMgr ${SOURCES})
target_link_libraries(LunaSysMgr
    ${LS2_LIBRARIES}
    ${GLIB2_LIBRARIES}
    ${GTHREAD2_LIBRARIES}
    ${LUNA_SYSMGR_COMMON_LIBRARIES}
    ${LUNA_SYSMGR_IPC_LIBRARIES}
    ${LUNA_PREFS_LIBRARIES}
    ${NYX_LIBRARIES}
    ${PBNJSON_C_LIBRARIES}
    ${PBNJSON_CPP_LIBRARIES}
    ${SQLITE3_LIBRARIES}
    ${CJSON_LIBRARIES}
    ${OPENSSL_LIBRARIES}
    ${SERVICEINSTALLER_LIBRARIES}
    ${PMLOGLIB_LIBRARIES}
    ${ROLEGEN_LIBRARIES}
    pthread)
qt5_use_modules(LunaSysMgr Core Gui Widgets)

webos_build_daemon()
webos_build_system_bus_files()
webos_config_build_doxygen(doc Doxyfile)