set(ProjectName "NFLuaScriptPlugin")
file(GLOB NFLuaScriptPlugin_ROOT_Cpp *.cpp)
file(GLOB NFLuaScriptPlugin_ROOT_CC *.cc)
file(GLOB NFLuaScriptPlugin_ROOT_Hpp *.h)
file(GLOB LUA_ROOT_C ${SolutionDir}/Dependencies/lua/*.c)
file(GLOB LUA_ROOT_H ${SolutionDir}/Dependencies/lua/*.h)

add_library(NFLuaScriptPlugin SHARED
	${NFLuaScriptPlugin_ROOT_Cpp}
	${NFLuaScriptPlugin_ROOT_CC}
	${NFLuaScriptPlugin_ROOT_Hpp}
	${LUA_ROOT_C}
	${LUA_ROOT_H})
	
if(UNIX)
	if (CMAKE_BUILD_TYPE MATCHES "Release")
		target_link_libraries(NFLuaScriptPlugin 
		#"-Wl,--whole-archive" 
		#liblua.a 
		#"-Wl,--no-whole-archive"
		)
	else()
                target_link_libraries(NFLuaScriptPlugin 
		#"-Wl,--whole-archive" 
		#liblua.a 
		#"-Wl,--no-whole-archive" 
                )
	endif()
	add_definitions(
		-D_USRDLL
		-D_LIB
		-DPROTOBUF_USE_DLLS
		-DNFDATABASEPLUGIN_EXPORTS
		-DWIN32_LEAN_AND_MEAN
		)
else()
	add_dependencies(NFLuaScriptPlugin libprotobuf NFMessageDefine)
	target_link_libraries(NFLuaScriptPlugin anet_win64 libprotobuf Theron NFMessageDefine)
	add_definitions(
		-D_USRDLL
		-D_LIB
		-DPROTOBUF_USE_DLLS
		-DNFDATABASEPLUGIN_EXPORTS
		-DWIN32_LEAN_AND_MEAN
		)
endif()
set_target_properties( NFLuaScriptPlugin PROPERTIES OUTPUT_NAME_DEBUG "NFLuaScriptPlugin_d")
set_target_properties( NFLuaScriptPlugin PROPERTIES PREFIX "")
set_target_properties( NFLuaScriptPlugin PROPERTIES
	FOLDER "NFSDK"
	ARCHIVE_OUTPUT_DIRECTORY ${NFOutPutDir}
	RUNTIME_OUTPUT_DIRECTORY ${NFOutPutDir}
	LIBRARY_OUTPUT_DIRECTORY ${NFOutPutDir} )
