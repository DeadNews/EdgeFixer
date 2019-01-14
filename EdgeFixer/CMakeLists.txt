project(EdgeFixer C)

################################################################################
# Source groups
################################################################################
set(Header_Files
    edgefixer.h
    VapourSynth.h
    VSHelper.h
)
source_group("Header Files" FILES ${Header_Files})

set(Source_Files
    edgefixer.c
    vsplugin.c
)
source_group("Source Files" FILES ${Source_Files})

set(ALL_FILES  ${Source_Files} ${Header_Files})

################################################################################
# Target
################################################################################
add_library(${PROJECT_NAME} SHARED ${ALL_FILES})

if(NOT ("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "Win32"
     OR "${CMAKE_VS_PLATFORM_NAME}" STREQUAL "x64"))
    message(WARNING "${CMAKE_VS_PLATFORM_NAME} arch is not supported!")
endif()

################################################################################
# Target name
################################################################################
if("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "Win32")
    string(CONCAT TARGET_NAME
          $<$<CONFIG:Debug>:${PROJECT_NAME}>
        $<$<CONFIG:Release>:${PROJECT_NAME}>
        $<$<NOT:$<OR:$<CONFIG:Debug>,$<CONFIG:Release>>>:${PROJECT_NAME}>
    )
elseif("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "x64")
    string(CONCAT TARGET_NAME
          $<$<CONFIG:Debug>:${PROJECT_NAME}>
        $<$<CONFIG:Release>:${PROJECT_NAME}>
        $<$<NOT:$<OR:$<CONFIG:Debug>,$<CONFIG:Release>>>:${PROJECT_NAME}>
    )
endif()
set_target_properties(${PROJECT_NAME} PROPERTIES OUTPUT_NAME ${TARGET_NAME})

################################################################################
# Output directory
################################################################################
if("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "Win32")
    string(CONCAT OUT_DIR
          $<$<CONFIG:Debug>:${CMAKE_SOURCE_DIR}/${CMAKE_VS_PLATFORM_NAME}/$<CONFIG>/>
        $<$<CONFIG:Release>:${CMAKE_SOURCE_DIR}/${CMAKE_VS_PLATFORM_NAME}/$<CONFIG>/>
        $<$<NOT:$<OR:$<CONFIG:Debug>,$<CONFIG:Release>>>:${CMAKE_SOURCE_DIR}/${CMAKE_VS_PLATFORM_NAME}/$<CONFIG>>
    )
elseif("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "x64")
    string(CONCAT OUT_DIR
          $<$<CONFIG:Debug>:${CMAKE_SOURCE_DIR}/${CMAKE_VS_PLATFORM_NAME}/$<CONFIG>/>
        $<$<CONFIG:Release>:${CMAKE_SOURCE_DIR}/${CMAKE_VS_PLATFORM_NAME}/$<CONFIG>/>
        $<$<NOT:$<OR:$<CONFIG:Debug>,$<CONFIG:Release>>>:${CMAKE_SOURCE_DIR}/${CMAKE_VS_PLATFORM_NAME}/$<CONFIG>>
    )
endif()
set(ARCHIVE_OUT_DIR ${OUT_DIR})
set(ARCHIVE_OUT_NAME ${PROJECT_NAME})
set_target_properties(${PROJECT_NAME} PROPERTIES ARCHIVE_OUTPUT_NAME ${ARCHIVE_OUT_NAME})
set_target_properties(${PROJECT_NAME} PROPERTIES ARCHIVE_OUTPUT_DIRECTORY ${ARCHIVE_OUT_DIR})
set_target_properties(${PROJECT_NAME} PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${OUT_DIR})
set_target_properties(${PROJECT_NAME} PROPERTIES LIBRARY_OUTPUT_DIRECTORY ${OUT_DIR})

################################################################################
# Include directories
################################################################################
if("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "Win32")
    target_include_directories(${PROJECT_NAME} PUBLIC
          $<$<CONFIG:Debug>:${CMAKE_CURRENT_SOURCE_DIR}/.>
    )
elseif("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "x64")
    target_include_directories(${PROJECT_NAME} PUBLIC
          $<$<CONFIG:Debug>:${CMAKE_CURRENT_SOURCE_DIR}/.>
    )
endif()

################################################################################
# Compile definitions
################################################################################

################################################################################
# Compile and link options
################################################################################
if(MSVC)
    if("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "Win32")
        target_compile_options(${PROJECT_NAME} PRIVATE
              $<$<CONFIG:Debug>:/Od;/MTd;/W3;/EHsc>
            $<$<CONFIG:Release>:/GL;/O2;/Oi;/MT;/Gy;/W3;/EHsc>
        )
    elseif("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "x64")
        target_compile_options(${PROJECT_NAME} PRIVATE
              $<$<CONFIG:Debug>:/Od;/MTd;/W3;/EHsc>
            $<$<CONFIG:Release>:/GL;/O2;/Oi;/MT;/Gy;/W3;/EHsc>
        )
    endif()
    if("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "Win32")
        set_target_properties(${PROJECT_NAME} PROPERTIES LINK_FLAGS_DEBUG "/DEBUG")
        set_target_properties(${PROJECT_NAME} PROPERTIES LINK_FLAGS_RELEASE "/LTCG /DEBUG")
    elseif("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "x64")
        set_target_properties(${PROJECT_NAME} PROPERTIES LINK_FLAGS_DEBUG "/DEBUG")
        set_target_properties(${PROJECT_NAME} PROPERTIES LINK_FLAGS_RELEASE "/LTCG /DEBUG")
    endif()
endif()

