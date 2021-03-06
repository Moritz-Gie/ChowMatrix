cmake_minimum_required(VERSION 3.15)
set(CMAKE_OSX_DEPLOYMENT_TARGET "10.12" CACHE STRING "Minimum OS X deployment target")
project(ChowMatrix VERSION 1.1.0)

set(CMAKE_CXX_STANDARD 17)
add_subdirectory(modules)
include_directories(modules)

# juce_set_vst2_sdk_path(C:/SDKs/VST_SDK/VST2_SDK/)
# include_directories(C:/SDKs/asiosdk2.3/common)

juce_add_plugin(ChowMatrix
    COMPANY_NAME chowdsp
    PLUGIN_MANUFACTURER_CODE Chow
    PLUGIN_CODE spg3
    FORMATS AU VST3 Standalone LV2 #VST
    ProductName "ChowMatrix"
    LV2_URI https://github.com/Chowdhury-DSP/ChowMatrix
    ICON_BIG res/logo.png
)

# create JUCE header
juce_generate_juce_header(ChowMatrix)

# add sources
add_subdirectory(src)
include_directories(src)
add_subdirectory(res)

target_compile_definitions(ChowMatrix
    PUBLIC
    JUCE_DISPLAY_SPLASH_SCREEN=0
    JUCE_REPORT_APP_USAGE=0
    JUCE_WEB_BROWSER=0
    JUCE_USE_CURL=0
    JUCE_VST3_CAN_REPLACE_VST2=0
)

target_link_libraries(ChowMatrix PUBLIC
    juce_plugin_modules
)

# we need these flags for notarization on MacOS
option(MACOS_RELEASE "Set build flags for MacOS Release" OFF)
if(MACOS_RELEASE)
    message(STATUS "Setting MacOS release flags...")
    set_target_properties(ChowMatrix_Standalone PROPERTIES
        XCODE_ATTRIBUTE_ENABLE_HARDENED_RUNTIME YES)
endif()
