# This is the CMake script for compiling a CGAL application.

project( Arrangement_on_surface_2_Demo )

cmake_minimum_required(VERSION 2.8.11)
if(POLICY CMP0053)
  cmake_policy(SET CMP0053 OLD)
endif()
if(POLICY CMP0043)
  cmake_policy(SET CMP0043 OLD)
endif()

find_package(CGAL COMPONENTS Core Qt5 )
include( ${CGAL_USE_FILE} )

find_package( Qt5 QUIET COMPONENTS Script Widgets )

if ( CGAL_FOUND AND CGAL_Qt5_FOUND AND Qt5_FOUND AND CGAL_Core_FOUND)
  include_directories( ./ )
  # Arrangement package includes
  include_directories( BEFORE ../../include )

  add_definitions(-DQT_NO_KEYWORDS)

  qt5_wrap_ui( arrangement_2_uis
    ArrangementDemoWindow.ui
    NewTabDialog.ui
    OverlayDialog.ui
    ArrangementDemoPropertiesDialog.ui
  )
  qt5_wrap_cpp( CGAL_Qt5_MOC_FILES
    ArrangementDemoWindow.h
    ArrangementDemoTab.h
    Callback.h
#    NewTabDialog.h
    OverlayDialog.h
    ArrangementDemoPropertiesDialog.h
    ColorItemEditor.h
    DeleteCurveModeItemEditor.h
    PropertyValueDelegate.h
    #PropertyValueDelegate.cpp
  )
  qt5_add_resources( CGAL_Qt5_RESOURCE_FILES
    ArrangementDemoWindow.qrc
  )
  add_executable( arrangement_2
    arrangement_2.cpp
    ArrangementGraphicsItem.cpp
    ArrangementDemoWindow.cpp
    ArrangementDemoTab.cpp
    ArrangementDemoGraphicsView.cpp
    Callback.cpp
    VerticalRayShootCallback.cpp
    EnvelopeCallback.cpp
    SplitEdgeCallback.cpp
    FillFaceCallback.cpp
    GraphicsViewSegmentInput.cpp
    GraphicsViewCurveInput.cpp
    Utils.cpp
    NewTabDialog.cpp
    OverlayDialog.cpp
    ArrangementDemoPropertiesDialog.cpp
    ColorItemEditor.cpp
    PropertyValueDelegate.cpp
    DeleteCurveMode.cpp
    DeleteCurveModeItemEditor.cpp
    PointsGraphicsItem.cpp
    VerticalRayGraphicsItem.cpp
   ${CGAL_Qt5_MOC_FILES}
   ${arrangement_2_uis}
   ${CGAL_Qt5_RESOURCE_FILES}
  )
  qt5_use_modules(arrangement_2 Widgets Script)
  target_link_libraries( arrangement_2
    ${CGAL_LIBRARIES}
    ${CGAL_3RD_PARTY_LIBRARIES}
    ${QT_LIBRARIES}
  )

  add_to_cached_list( CGAL_EXECUTABLE_TARGETS arrangement_2 )

else()

  message(STATUS "NOTICE: This demo requires CGAL, CGAL-Core and Qt5, and will not be compiled.")

endif()

# add_subdirectory( tests )
