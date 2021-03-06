# cmake config for every language subdirectory.
# Just copy it to new language subdirectory to get it working
#
GET_FILENAME_COMPONENT(lang ${CMAKE_CURRENT_BINARY_DIR} NAME)
MESSAGE(STATUS "Building for: ${lang}")

SET (install_path ${CMAKE_INSTALL_PREFIX}/${lang}/)
MESSAGE(STATUS "Install Path: ${install_path}")
SET (docbook_index index.docbook)


ADD_CUSTOM_TARGET(docbook ALL
    ${xsltcmd} --output ${install_path}
          --stringparam html.stylesheet ${cssstyle}
          --stringparam chunker.output.encoding UTF-8
          ${xslstyle}
          ${CMAKE_CURRENT_SOURCE_DIR}/${docbook_index}
    DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/${docbook_index}
    COMMENT "processing ${docbook_index}"
    VERBATIM
)

INSTALL(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/..
        DESTINATION ${install_path}
        USE_SOURCE_PERMISSIONS
          PATTERN ".svn" EXCLUDE
          PATTERN "CMakeFiles" EXCLUDE
          PATTERN "Makefile" EXCLUDE
          PATTERN "cmake_install.cmake" EXCLUDE
          PATTERN "CMakeCache.txt" EXCLUDE
          PATTERN "install_manifest.txt" EXCLUDE
          PATTERN "*.html"
)

INSTALL(DIRECTORY ${CMAKE_SOURCE_DIR}/common/
        DESTINATION ${install_path}
        USE_SOURCE_PERMISSIONS
          PATTERN ".svn" EXCLUDE
          PATTERN "*.*"
)

INSTALL(DIRECTORY ${CMAKE_SOURCE_DIR}/${lang}/
        DESTINATION ${install_path}
        USE_SOURCE_PERMISSIONS
          PATTERN ".svn" EXCLUDE
          PATTERN "*.docbook" EXCLUDE
          PATTERN "CMakeLists.txt" EXCLUDE
          PATTERN "*.*"
)

#icons
INSTALL(DIRECTORY ${CMAKE_SOURCE_DIR}/../Sqliteman/sqliteman/icons/
        DESTINATION ${install_path}
        USE_SOURCE_PERMISSIONS
          PATTERN ".svn" EXCLUDE
          PATTERN "CMakeFiles" EXCLUDE
          PATTERN "Makefile" EXCLUDE
          PATTERN "cmake_install.cmake" EXCLUDE
          PATTERN "CMakeCache.txt" EXCLUDE
          PATTERN "CMakeLists.txt" EXCLUDE
          PATTERN "install_manifest.txt" EXCLUDE
          PATTERN "*.svg" EXCLUDE
          PATTERN "*rc" EXCLUDE
          PATTERN "AUTHORS" EXCLUDE
          PATTERN "*.png"
)
