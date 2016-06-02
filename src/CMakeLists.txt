

SET(KIT_SRCS
  qtbuttonpropertybrowser.cpp
  qtbuttonpropertybrowser.h
  qteditorfactory.cpp
  qteditorfactory.h
  qtgroupboxpropertybrowser.cpp
  qtgroupboxpropertybrowser.h
  qtpropertybrowser.cpp
  qtpropertybrowser.h
  qtpropertybrowserutils.cpp
  qtpropertybrowserutils_p.h
  qtpropertymanager.cpp
  qtpropertymanager.h
  qttreepropertybrowser.cpp
  qttreepropertybrowser.h
  qtvariantproperty.cpp
  qtvariantproperty.h
  )

SET(KIT_MOC_SRCS
  qtpropertybrowserutils_p.h
  )

SET(KIT_UI_FORMS
  )

SET(KIT_resources
  qtpropertybrowser.qrc
  )
  
INCLUDE_DIRECTORIES(
  ${CMAKE_CURRENT_SOURCE_DIR}
  ${CMAKE_CURRENT_BINARY_DIR}
  )
  
QT5_WRAP_UI(KIT_UI_CPP ${KIT_UI_FORMS})
QT5_WRAP_CPP(KIT_MOC_CPP ${KIT_MOC_SRCS})
QT5_ADD_RESOURCES(KIT_QRC_SRCS ${KIT_resources})

SET(libname ${PROJECT_NAME})
ADD_LIBRARY(${libname}
  ${KIT_SRCS}
  ${KIT_UI_CPP}
  ${KIT_MOC_CPP}
  ${KIT_QRC_SRCS}
  )

SET_TARGET_PROPERTIES(${libname} PROPERTIES INSTALL_NAME_DIR "${CMAKE_INSTALL_PREFIX}/lib")

SET(${PROJECT_NAME}_LINK_LIBRARIES ${Qt5Widgets_LIBRARIES})
TARGET_LINK_LIBRARIES(
  ${libname}
  ${${PROJECT_NAME}_LINK_LIBRARIES}
  )

# List of header that should go though moc
SET(KIT_HEADERS_MOC_SRCS
  qtbuttonpropertybrowser.h
  qteditorfactory.h
  qtgroupboxpropertybrowser.h
  qtpropertybrowser.h
  qtpropertybrowserutils_p.h
  qtpropertymanager.h
  qttreepropertybrowser.h
  qtvariantproperty.h
  )

SET(KIT_MOC_CPP)
FOREACH(file ${KIT_HEADERS_MOC_SRCS})
  get_filename_component(filename_we ${file} NAME_WE)
  SET(output_file moc_${filename_we}.cpp)
  QT5_GENERATE_MOC(${file} ${output_file})
  LIST(APPEND KIT_MOC_CPP ${output_file})
ENDFOREACH()

# List of cpp files that should go though moc
SET(KIT_CPP_MOC_SRCS
  qteditorfactory.cpp
  qtpropertymanager.cpp
  qttreepropertybrowser.cpp
  )

FOREACH(file ${KIT_CPP_MOC_SRCS})
  get_filename_component(filename_we ${file} NAME_WE)
  SET(output_file ${filename_we}.moc)
  QT5_GENERATE_MOC(${file} ${output_file})
  LIST(APPEND KIT_MOC_CPP ${output_file})
ENDFOREACH()

add_custom_target(${libname}GenerateMoc
  DEPENDS ${KIT_MOC_CPP}
  )
add_dependencies(${libname} ${libname}GenerateMoc)

install(FILES
  ${KIT_HEADERS_MOC_SRCS}
  QtAbstractEditorFactoryBase
  QtAbstractPropertyBrowser
  QtAbstractPropertyManager
  QtBoolPropertyManager
  QtBrowserItem
  QtButtonPropertyBrowser
  QtCharEditorFactory
  QtCharPropertyManager
  QtCheckBoxFactory
  QtColorEditorFactory
  QtColorPropertyManager
  QtCursorEditorFactory
  QtCursorPropertyManager
  QtDateEditFactory
  QtDatePropertyManager
  QtDateTimeEditFactory
  QtDateTimePropertyManager
  QtDoublePropertyManager
  QtDoubleSpinBoxFactory
  QtEnumEditorFactory
  QtEnumPropertyManager
  QtFlagPropertyManager
  QtFontEditorFactory
  QtFontPropertyManager
  QtGroupBoxPropertyBrowser
  QtGroupPropertyManager
  QtIntPropertyManager
  QtKeySequenceEditorFactory
  QtKeySequencePropertyManager
  QtLineEditFactory
  QtLocalePropertyManager
  QtPointFPropertyManager
  QtPointPropertyManager
  QtProperty
  QtRectFPropertyManager
  QtRectPropertyManager
  QtScrollBarFactory
  QtSizeFPropertyManager
  QtSizePolicyPropertyManager
  QtSizePropertyManager
  QtSliderFactory
  QtSpinBoxFactory
  QtStringPropertyManager
  QtTimeEditFactory
  QtTimePropertyManager
  QtTreePropertyBrowser
  QtVariantEditorFactory
  QtVariantProperty
  QtVariantPropertyManager
  DESTINATION include/QtPropertyBrowser)

install(TARGETS ${libname}
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib)
