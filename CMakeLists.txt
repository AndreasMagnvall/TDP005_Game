project(sfml)

# Anv�nd C++ 17.
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17")

# P� skolans Linux-system finns tv� versioner av GCC. Se vilken som anv�nds:
if (DEFINED ENV{LOADEDMODULES} AND ("$ENV{LOADEDMODULES}" MATCHES ".*prog/gcc/6.1.0.*"))
    set(CMAKE_CXX_COMPILER /sw/gcc-6.1.0/bin/g++)

    if (DEFINED ENV{SFML_ROOT})
        list(APPEND CMAKE_MODULE_PATH "$ENV{SFML_ROOT}/share/SFML/cmake/Modules")
    endif()
endif()

if (DEFINED ENV{LOADEDMODULES} AND ("$ENV{LOADEDMODULES}" MATCHES ".*prog/gcc/7.1.0.*"))
    set(CMAKE_CXX_COMPILER /sw/gcc-7.1.0/bin/g++)

    if (DEFINED ENV{SFML_ROOT})
        list(APPEND CMAKE_MODULE_PATH "$ENV{SFML_ROOT}/share/SFML/cmake/Modules")
    endif()
endif()

# V�lj vilka delar av SFML som ska anv�ndas.
set(SFML_MODULES network graphics window system)

# F�rs�k att hitta SFML genom att anropa 'FindSFML.cmake'.
find_package(SFML 2 COMPONENTS ${SFML_MODULES})
include_directories(${SFML_INCLUDE_DIR})

# Om det inte gick hoppas vi att det �r installerat i alla fall.
if (NOT (${SFML_FOUND} STREQUAL "TRUE"))
    set(SFML_LIBRARIES "")
    foreach(i ${SFML_MODULES})
        list(APPEND SFML_LIBRARIES "sfml-${i}")
    endforeach(i)
endif()

# Ange k�llfiler, l�gg till de filer som ni beh�ver!
set(SOURCE_FILES 
                 Main.cpp
                 source/Game.cpp
                 source/Object.cpp
                 source/State.cpp
                 source/Tile.cpp
                 source/Floor.cpp
                 source/Play_State.cpp
                 source/Menu_State.cpp
                 source/Movable_Obj.cpp
                 source/Character.cpp
                 source/Player.cpp
                 source/Bullet.cpp
                 source/Wall.cpp
                 source/Enemy.cpp
                 source/GameMap.cpp
                 source/Enemy_Spawner.cpp
                 source/UI.cpp
                # klass.cc
                # klass.h
      )

# S�g till CMake att skapa en k�rbar fil som heter 'sfml'.
add_executable(sfml ${SOURCE_FILES})

# L�nka med biblioteken som vi hittade tidigare.
target_link_libraries(sfml ${SFML_LIBRARIES} ${SFML_DEPENDENCIES})
