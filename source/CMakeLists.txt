## FiberTaskingLib - A tasking library that uses fibers for efficient task switching
 #
 # This library was created as a proof of concept of the ideas presented by
 # Christian Gyrling in his 2015 GDC Talk 'Parallelizing the Naughty Dog Engine Using Fibers'
 #
 # http://gdcvault.com/play/1022186/Parallelizing-the-Naughty-Dog-Engine
 #
 # FiberTaskingLib is the legal property of Adrian Astley
 # Copyright Adrian Astley 2015 - 2016
 ##

include(SetSourceGroup)

# Unix systems need to be explicitly linked to the threading lib
if (UNIX)
	find_package(Threads REQUIRED)
endif()

SetSourceGroup(NAME Core
	PREFIX FTL
	SOURCE_FILES 
				 ../include/fiber_tasking_lib/task.h
	             ../include/fiber_tasking_lib/task_scheduler.h
				 ../include/fiber_tasking_lib/typedefs.h
	             task_scheduler.cpp
)

SetSourceGroup(NAME Util
	PREFIX FTL
	SOURCE_FILES ../include/fiber_tasking_lib/config.h
	             ../include/fiber_tasking_lib/fiber.h
	             ../include/fiber_tasking_lib/thread_abstraction.h
	             ../include/fiber_tasking_lib/thread_safe_queue.h
)

# Link all the sources into one
set(FIBER_TASKING_LIB_SRC
	${FTL_CORE}
	${FTL_UTIL}
)

add_library(FiberTaskingLib STATIC ${FIBER_TASKING_LIB_SRC})
target_link_libraries(FiberTaskingLib boost_context ${CMAKE_THREAD_LIBS_INIT})
target_include_directories(FiberTaskingLib PUBLIC ../include)

# Remove the prefix
set_target_properties(FiberTaskingLib PROPERTIES PREFIX "")
