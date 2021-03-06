# This is basically the overall name of the project in Visual Studio this is the name of the Solution File


# For every executable you have with a main method you should have an add_executable line below.
# For every add executable line you should list every .cpp and .h file you have associated with that executable.  



# You shouldn't have to modify anything below this line 
########################################################


INCLUDE_DIRECTORIES(
${BULLET_PHYSICS_SOURCE_DIR}/src 
../OpenGL
${BULLET_PHYSICS_SOURCE_DIR}/src ../OpenGL ../../btgui
${GLUT_INCLUDE_DIR}
${BULLET2_TBB_INCLUDE_DIR}
)

IF(WIN32)
	ADD_DEFINITIONS(-DGLEW_STATIC)
ENDIF(WIN32)

LINK_LIBRARIES(
OpenGLSupport   BulletDynamics  BulletCollision LinearMath    ${GLUT_glut_LIBRARY} ${OPENGL_gl_LIBRARY} ${OPENGL_glu_LIBRARY}
)

ADD_EXECUTABLE(AppMultiThreadedDemo
	main.cpp
	MultiThreadedDemo.cpp
	MultiThreadedDemo.h
)

IF (UNIX)
  TARGET_LINK_LIBRARIES(AppMultiThreadedDemo pthread)
ENDIF (UNIX)

TARGET_LINK_LIBRARIES(AppMultiThreadedDemo ${TBB_LIBRARY} ${TBBMALLOC_LIBRARY})


IF (INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)
			SET_TARGET_PROPERTIES(AppMultiThreadedDemo PROPERTIES  DEBUG_POSTFIX "_Debug")
			SET_TARGET_PROPERTIES(AppMultiThreadedDemo PROPERTIES  MINSIZEREL_POSTFIX "_MinsizeRel")
			SET_TARGET_PROPERTIES(AppMultiThreadedDemo PROPERTIES  RELWITHDEBINFO_POSTFIX "_RelWithDebugInfo")
ENDIF(INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)

IF(WIN32)
	set(TBB_VC_VER "vc12")
	set(TBB_VC_ARCH "ia32")
	# assume 32-bit build in VC12 for now
	# checks can be added here at a later time
	
	ADD_CUSTOM_COMMAND(TARGET AppMultiThreadedDemo POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
        "${BULLET2_TBB_INCLUDE_DIR}/../bin/${TBB_VC_ARCH}/${TBB_VC_VER}/tbb.dll"
        $<TARGET_FILE_DIR:AppMultiThreadedDemo>)
ENDIF(WIN32)