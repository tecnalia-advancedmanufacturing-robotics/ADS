set(LIBRARY_NAME ads)

list(APPEND HEADERS
  AdsDef.h
  AdsDevice.h
  AdsFile.h
  AdsLib.h
  AdsNotification.h
  AdsNotificationOOI.h
  AdsVariable.h
  AmsConnection.h
  AmsHeader.h
  AmsRouter.h
  Frame.h
  LicenseAccess.h
  Log.h
  NotificationDispatcher.h
  RingBuffer.h
  Router.h
  RouterAccess.h
  RTimeAccess.h
  Sockets.h
  wrap_endian.h
  wrap_socket.h

  standalone/AdsDef.h
  standalone/AdsLib.h
)

list(APPEND SOURCES
  AdsDef.cpp
  AdsDevice.cpp
  AdsFile.cpp
  AdsLib.cpp
  Frame.cpp
  LicenseAccess.cpp
  Log.cpp
  RouterAccess.cpp
  RTimeAccess.cpp
  Sockets.cpp

  standalone/AdsLib.cpp
  standalone/AmsConnection.cpp
  standalone/AmsNetId.cpp
  standalone/AmsPort.cpp
  standalone/AmsRouter.cpp
  standalone/NotificationDispatcher.cpp
)

# Define the target and alias
add_library(${LIBRARY_NAME} ${SOURCES})
add_library(${PROJECT_NAME}::${LIBRARY_NAME} ALIAS ${LIBRARY_NAME})

# Add target libraries for the compiler (.._include_..) and the linker (.._link_..)
include(GNUInstallDirs)
target_include_directories(${LIBRARY_NAME} PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
  $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

if(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
  target_link_libraries(${LIBRARY_NAME} PUBLIC wsock32)
endif()

if(WIN32 EQUAL 1)
    target_link_libraries(ads PUBLIC ws2_32)
endif()

target_link_libraries(${LIBRARY_NAME} PUBLIC Threads::Threads)

# Install rules
install(TARGETS ${LIBRARY_NAME}
  EXPORT ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
)
install(FILES ${HEADERS}
  DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/${LIBRARY_NAME}
)
if(CMAKE_SYSTEM_NAME STREQUAL "Windows")
  if(BUILD_SHARED_LIBS)
    install(TARGETS ${LIBRARY_NAME}
      LIBRARY DESTINATION ${CMAKE_INSTALL_BINDIR}
    )
  endif()
endif()
