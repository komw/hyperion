# Configure minimum CMAKE version
cmake_minimum_required(VERSION 2.8)

# Set the project name
project(hyperion-x11)

# find QT
if(ENABLE_QT5)
	find_package(Qt5Widgets REQUIRED)
else(ENABLE_QT5)
	find_package(Qt4 REQUIRED QtCore QtGui QtNetwork)
endif(ENABLE_QT5)

# Find X11
find_package(X11 REQUIRED)

include_directories(
	${CMAKE_CURRENT_BINARY_DIR}/../../libsrc/protoserver
	${QT_INCLUDES}
	${X11_INCLUDES}
	${PROTOBUF_INCLUDE_DIRS}
)

set(Hyperion_X11_QT_HEADERS
	X11Wrapper.h)

set(Hyperion_X11_HEADERS
)

set(Hyperion_X11_SOURCES
	hyperion-x11.cpp
	X11Wrapper.cpp
)

if(ENABLE_QT5)
	QT5_WRAP_CPP(Hyperion_X11_HEADERS_MOC ${Hyperion_X11_QT_HEADERS})
else(ENABLE_QT5)
	QT4_WRAP_CPP(Hyperion_X11_HEADERS_MOC ${Hyperion_X11_QT_HEADERS})
endif(ENABLE_QT5)


add_executable(${PROJECT_NAME}
	${Hyperion_X11_HEADERS}
	${Hyperion_X11_SOURCES}
	${Hyperion_X11_HEADERS_MOC}
)

target_link_libraries(${PROJECT_NAME}
	getoptPlusPlus
	blackborder
	hyperion-utils
	protoserver
	x11-grabber
	${X11_LIBRARIES}
	pthread
)

if(ENABLE_QT5)
	qt5_use_modules(${PROJECT_NAME} Widgets Core Gui Network)
else(ENABLE_QT5)
	qt4_use_modules(${PROJECT_NAME} Core Gui Network )
endif(ENABLE_QT5)
