cmake_minimum_required(VERSION 3.12)
project(rigidBodyTutorial)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

if(NOT CMAKE_BUILD_TYPE)
	message(STATUS "No build type selected, default to Release")
	set(CMAKE_BUILD_TYPE "Release")
endif()

# Find OpenGL
find_package(OpenGL REQUIRED)

# Find Eigen
find_package(Eigen3 REQUIRED)

# Find Polyscope
find_package(Polyscope REQUIRED)
 
set(collision_HEADERS include/collision/CollisionDetect.h 
                      include/collision/Geometry.h )
set(collision_SOURCE src/collision/CollisionDetect.cpp ) 

set(rigidbody_HEADERS include/rigidbody/RigidBody.h
                      include/rigidbody/RigidBodySystem.h 
                      include/rigidbody/Scenarios.h )
set(rigidbody_SOURCE src/rigidbody/RigidBody.cpp 
                     src/rigidbody/RigidBodySystem.cpp )

set(contact_HEADERS include/contact/Contact.h )
set(contact_SOURCE src/contact/Contact.cpp )

set(solver_HEADERS include/solvers/Solver.h 
                   include/solvers/SolverBoxPGS.h )
set(solver_SOURCE src/solvers/SolverBoxPGS.cpp )

set(util_HEADERS include/util/OBJLoader.h
                 include/util/MeshAssets.h )
set(util_SOURCE src/util/OBJLoader.cpp
                src/util/MeshAssets.cpp )

set(viewer_HEADERS include/viewer/SimViewer.h)
set(viewer_SOURCE src/viewer/SimViewer.cpp)
        
add_executable (${PROJECT_NAME} main.cpp
          ${contact_HEADERS} ${contact_SOURCE} 
          ${collision_HEADERS} ${collision_SOURCE}
          ${rigidbody_HEADERS} ${rigidbody_SOURCE}
          ${solver_HEADERS} ${solver_SOURCE}
		  ${util_HEADERS} ${util_SOURCE}
          ${viewer_HEADERS} ${viewer_SOURCE} )
target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include ${EIGEN3_INCLUDE_DIR} ${COMMON_INCLUDES})
target_link_libraries(${PROJECT_NAME} OpenGL::GL polyscope)

# Setup source code groups
#
source_group(src FILES main.cpp MainWindow.cpp)
source_group(include FILES MainWindow.h)
source_group(ui FILES main.ui)
source_group(src/collision FILES ${collision_SOURCE})
source_group(include/collision FILES ${collision_HEADERS})
source_group(src/contact FILES ${contact_SOURCE})
source_group(include/contact FILES ${contact_HEADERS})
source_group(src/rigidbody FILES ${rigidbody_SOURCE})
source_group(include/rigidbody FILES ${rigidbody_HEADERS})
source_group(src/solvers FILES ${solver_SOURCE})
source_group(include/solvers FILES ${solver_HEADERS})
source_group(src/util FILES ${util_SOURCE})
source_group(include/util FILES ${util_HEADERS})
source_group(src/viewer FILES ${viewer_SOURCE})
source_group(include/viewer FILES ${viewer_HEADERS})
source_group(src/util FILES ${util_SOURCE})

if(MSVC) 
	set_property(TARGET ${PROJECT_NAME} PROPERTY VS_DEBUGGER_WORKING_DIRECTORY ${CMAKE_SOURCE_DIR})
endif()