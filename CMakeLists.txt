cmake_minimum_required (VERSION 3.16 FATAL_ERROR)

# KDE Application Version, managed by release script
set(RELEASE_SERVICE_VERSION_MAJOR "21")
set(RELEASE_SERVICE_VERSION_MINOR "08")
set(RELEASE_SERVICE_VERSION_MICRO "0")
set(RELEASE_SERVICE_VERSION "${RELEASE_SERVICE_VERSION_MAJOR}.${RELEASE_SERVICE_VERSION_MINOR}.${RELEASE_SERVICE_VERSION_MICRO}")

project(kcharselect VERSION ${RELEASE_SERVICE_VERSION})

set (QT_MIN_VERSION "5.15.0")
set (KF5_MIN_VERSION "5.83.0")

find_package (ECM ${KF5_MIN_VERSION} REQUIRED NO_MODULE)
set (CMAKE_MODULE_PATH ${ECM_MODULE_PATH})

include(KDEInstallDirs)
include(KDEFrameworkCompilerSettings NO_POLICY_SCOPE)
include(KDECMakeSettings)
include(ECMSetupVersion)

find_package (Qt5 ${QT_MIN_VERSION} CONFIG REQUIRED COMPONENTS
    Core
    Widgets
)

find_package(KF5 ${KF5_MIN_VERSION} REQUIRED COMPONENTS
    Bookmarks
    Crash
    DocTools
    I18n
    WidgetsAddons
    XmlGui
)

ecm_setup_version(${RELEASE_SERVICE_VERSION} VARIABLE_PREFIX KCHARSELECT VERSION_HEADER kcharselect_version.h)

add_executable(kcharselect)

target_sources(kcharselect PRIVATE
    kcharselectdia.cc
    main.cc

    kcharselect.qrc
)

target_link_libraries(kcharselect
    KF5::Bookmarks
    KF5::Crash
    KF5::I18n
    KF5::WidgetsAddons
    KF5::XmlGui
)

install(TARGETS kcharselect  ${KDE_INSTALL_TARGETS_DEFAULT_ARGS} )

install( PROGRAMS org.kde.kcharselect.desktop  DESTINATION ${KDE_INSTALL_APPDIR})
install(FILES org.kde.kcharselect.appdata.xml DESTINATION ${KDE_INSTALL_METAINFODIR})

add_subdirectory(doc)

feature_summary(WHAT ALL INCLUDE_QUIET_PACKAGES FATAL_ON_MISSING_REQUIRED_PACKAGES)
ki18n_install(po)
if (KF5DocTools_FOUND)
 kdoctools_install(po)
endif()
