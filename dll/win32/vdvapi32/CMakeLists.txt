
add_definitions(-D_ADVAPI32_)

remove_definitions(-D_WIN32_WINNT=0x502)
add_definitions(-D_WIN32_WINNT=0x600)
include_directories(${REACTOS_SOURCE_DIR}/include/reactos/wine)
include_directories(${REACTOS_SOURCE_DIR}/lib/rtl)
include_directories(${REACTOS_SOURCE_DIR}/dll/win32/ntdllnew)
include_directories(${REACTOS_SOURCE_DIR}/dll/win32/kernel32/include)

spec2def(vdvapi32.dll vdvapi32.spec ADD_IMPORTLIB)

list(APPEND SOURCE
    main.c
    version.rc
    ${CMAKE_CURRENT_BINARY_DIR}/vdvapi32_stubs.c
    ${CMAKE_CURRENT_BINARY_DIR}/vdvapi32.def)

add_library(vdvapi32 SHARED ${SOURCE})
set_module_type(vdvapi32 win32dll ENTRYPOINT 0)

target_link_libraries(vdvapi32 wine)

add_importlibs(vdvapi32 advapinew advapibase advapilong wevtapibase ntdll vtdll)

add_cd_file(TARGET vdvapi32 DESTINATION reactos/system32 FOR all)
