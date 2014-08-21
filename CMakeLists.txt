cmake_minimum_required(VERSION 2.8)

if(COMMAND cmake_policy)
    cmake_policy(SET CMP0021 OLD)
endif(COMMAND cmake_policy)

project(vehicle_environment)
#set path for local find modules
IF(${CMAKE_SYSTEM_NAME} MATCHES "Windows")
	set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake" "C:/Ogre/OgreSDK_vc10_v1-9-0/CMake")
ELSEIF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
ELSEIF(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
	set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake" "/usr/local/opt/ogre/CMake")
ENDIF()

IF ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
    SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}  -std=c++11")
ENDIF()

include(cmake/SBELUtils.cmake)
#enable_openmp_support()
find_package(OGRE REQUIRED)
find_package(SDL2 REQUIRED)
# User specified path to ChronoEngine
SET(CHRONO_INC           ""    CACHE   PATH    "Where is ChronoEngine Source located")
# User specified path to ChronoEngine Libraries
SET(CHRONO_LIB_PATH      ""   CACHE    PATH    "Where is ChronoEngine Lib located")
# Find required Libraries
FIND_LIBRARY(CHRONO_LIB NAMES ChronoEngine PATHS ${CHRONO_LIB_PATH} REQUIRED)

IF(${CMAKE_SYSTEM_NAME} MATCHES "Windows")
	IF (CMAKE_BUILD_TYPE EQUAL "Release")
		SET(LIBRARIES ${CHRONO_LIB} ${OGRE_LIBRARY_REL} ${Boost_THREAD_LIBRARY_RELEASE} ${SDL2_LIBRARY})
	ELSE (CMAKE_BUILD_TYPE EQUAL "Debug")
		SET(LIBRARIES ${CHRONO_LIB} ${OGRE_LIBRARY_DBG} ${Boost_THREAD_LIBRARY_DEBUG} ${SDL2_LIBRARY})
	ENDIF()
ELSEIF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
ELSEIF(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
		SET(OGRE_FRAMEWORKS ${OGRE_LIBRARY_REL} ${OGRE_Overlay_LIBRARY_REL} ${OGRE_Paging_LIBRARY_REL} ${OGRE_Property_LIBRARY_REL})
		SET(LIBRARIES ${CHRONO_LIB} ${OGRE_FRAMEWORKS} ${Boost_THREAD_LIBRARY_RELEASE} ${SDL2_LIBRARY})
ENDIF()



include_directories(
    ${CHRONO_INC}
	${OGRE_INCLUDE_DIR}
	${OGRE_INCLUDE_DIR}../
	${Boost_INCLUDE_DIR}
	${SDL2_INCLUDE_DIR}
    "${CHRONO_INC}/collision" 
    "${CHRONO_INC}/collision/bullet" 
    )
	SET(SOURCES 
		source/main.cpp
		source/ECApplication.cpp
        source/ECBody.cpp
        source/ECCameraManager.cpp
        source/ECGUIButton.cpp
        source/ECGUIElement.cpp
        source/ECGUIManager.cpp
        source/ECGUIPanel.cpp
        source/ECGUIText.cpp
        source/ECScene.cpp
        source/EC_SDL_InputManager.cpp
        source/VESuspensionDemo.cpp
        source/VEVehicle.cpp)

	SET(HEADERS 
		source/ECApplication.h
        source/ECBody.h
        source/ECCameraManager.h
        source/ECGUIButton.h
        source/ECGUIElement.h
        source/ECGUIManager.h
        source/ECGUIPanel.h
        source/ECGUIText.h
        source/ECScene.h
        source/EC_SDL_InputManager.h
        source/VESuspensionDemo.h
        source/VEVehicle.cpp
        source/VEVehicle.h )

	ADD_EXECUTABLE(vehicle_environment ${SOURCES} ${HEADERS})
	TARGET_LINK_LIBRARIES(vehicle_environment	${LIBRARIES})