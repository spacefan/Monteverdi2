###############################################################################
# SQL Queries.
#------------------------------------------------------------------------------

# SQL queries source files.
set( Monteverdi_SQL_FILES
  sql_db_create.sql
  sql_db_setup.sql
  sql_queries_insert.sql
  sql_queries_select.sql
)

# SQL C++ target file
set( Monteverdi_SQL_OUTPUT
  ${CMAKE_SOURCE_DIR}/Code/Common/Core/mvdSql.cxx
)

#
# Configuration option to enable/disable ?
option(
  GENERATE_SQL
  "Generate C++ source files from SQL queries source files. WARNING: 'make clean' will delete the source ${Monteverdi_SQL_OUTPUT} file!"
  OFF
)

if( GENERATE_SQL )
  message( AUTHOR_WARNING "WARNING: GENERATE_SQL option is ON. 'make clean' will delete the source ${Monteverdi_SQL_OUTPUT} file!" )

  # Create source list containing absolute filenames because custom commands
  # will be called from build dir.
  foreach( SOURCE_FILE ${Monteverdi_SQL_FILES} )
    get_filename_component( FILE ${SOURCE_FILE} ABSOLUTE )
    set( Monteverdi_SQL_SOURCES ${Monteverdi_SQL_SOURCES} ${FILE} )
  endforeach( SOURCE_FILE )

  # message( STATUS "SQL source files: ${Monteverdi_SQL_SOURCES}")
  # message( STATUS "SQL output file: ${Monteverdi_SQL_OUTPUT}")

  add_custom_target( Monteverdi_Sql
    ALL
    #${CMAKE_SOURCE_DIR}/Scripts/genc.sh -s .sql ${Monteverdi_SQL_SOURCES} > ${Monteverdi_SQL_OUTPUT}
    #COMMENT "Generating ${Monteverdi_SQL_OUTPUT}"
    DEPENDS ${Monteverdi_SQL_OUTPUT}
  )

  add_custom_command(
    OUTPUT ${Monteverdi_SQL_OUTPUT}
    DEPENDS ${Monteverdi_SQL_SOURCES}
    COMMENT "Generating ${Monteverdi_SQL_OUTPUT}"
    COMMAND ${CMAKE_SOURCE_DIR}/Scripts/genc.sh -s .sql ${Monteverdi_SQL_SOURCES} > ${Monteverdi_SQL_OUTPUT}
  )

  # add_custom_command(
  #   TARGET Monteverdi_Sql
  #   PRE_BUILD
  #   COMMENT "Generating ${Monteverdi_SQL_OUTPUT}"
  #   COMMAND "${CMAKE_SOURCE_DIR}/Scripts/genc.sh -s .sql ${Monteverdi_SQL_SOURCES} > ${Monteverdi_SQL_OUTPUT}"
  # )

endif( GENERATE_SQL )
