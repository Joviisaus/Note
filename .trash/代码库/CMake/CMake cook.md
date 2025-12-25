## 检测环境

### 操作系统相关
```cmake
if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
	message(STATUS "Configuring on/for Linux")
elseif(CMAKE_SYSTEM_NAME STREQUAL "Darwin")
	message(STATUS "Configuring on/for macOS")
elseif(CMAKE_SYSTEM_NAME STREQUAL "Windows")
	message(STATUS "Configuring on/for Windows")
elseif(CMAKE_SYSTEM_NAME STREQUAL "AIX")
	message(STATUS "Configuring on/for IBM AIX")
else()
	message(STATUS "Configuring on/for ${CMAKE_SYSTEM_NAME}")
endif()

# 可通过定义将信息传递给 cpp
target_compile_definitions(hello-world PUBLIC "IS_LINUX")
```

### 编译器相关
```cmake
target_compile_definitions(hello-world PUBLIC "COMPILER_NAME=\"${CMAKE_CXX_COMPILER_ID}\"")

if(CMAKE_CXX_COMPILER_ID MATCHES Intel)
	target_compile_definitions(hello-world PUBLIC "IS_INTEL_CXX_COMPILER")
endif()

if(CMAKE_CXX_COMPILER_ID MATCHES GNU)
	target_compile_definitions(hello-world PUBLIC "IS_GNU_CXX_COMPILER")
endif()

if(CMAKE_CXX_COMPILER_ID MATCHES PGI)
	target_compile_definitions(hello-world PUBLIC "IS_PGI_CXX_COMPILER")
endif()

if(CMAKE_CXX_COMPILER_ID MATCHES XL)
	target_compile_definitions(hello-world PUBLIC "IS_XL_CXX_COMPILER")
endif()
```

