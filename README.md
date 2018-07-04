[![GitHub release](https://img.shields.io/github/release/kenkit/ogre.svg)]()
[![Join the chat at https://gitter.im/OGRECave/ogre](https://badges.gitter.im/OGRECave/ogre.svg)](https://gitter.im/OGRECave/ogre?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![GitHub Download Count](https://github-basic-badges.herokuapp.com/downloads/kenkit/ogre/total.svg)](https://github.com/kenkit/ogre/releases)
[![GitHub issues](https://img.shields.io/github/issues-raw/kenkit/ogre.svg)]()
![](Docs/ogre-logo-wetfloor.gif)


# OGRE PREBUILT SDK VS2015
## Summary
**OGRE PREBUILT SDK**
(Object-Oriented Graphics Rendering Engine Prebuilt SDK) 
This is a prebuilt sdk of both ogre and cegui.It contains the necessary libs required to use Ogre+Cegui in your application. 
Builds are generated automatically and sourced from the respective repos.
The builds are also from the latest commits available.

I'm trying to avoid having to build both sdk locally as it's too huge and would take a long time to finish building.
Also not to forget the space+download data required to build it is alot.
So if you think this will be usefull there is a star icon somwewhere up there.

## Note
If you have any suggestions/changes you would like to see here, please use the issue tab.

## Build Log
The build log can be retrieved from here.

[2.1  log](https://ci.appveyor.com/project/kenkit/ogre-6fnyg?fullLog=true)

[1.11 log](https://ci.appveyor.com/project/kenkit/ogre-mm8lb?fullLog=true)

## Releases
[Download](https://github.com/kenkit/ogre/releases)

## AUTO UPDATE [CMAKE]
The following cmake script will download and install the latest ogre from this repo.
You must make sure you have set your OGRE_HOME Enviroment variable to your preffered instalation location.
This script will automatically set OGRE_DIR and SDL2_DIR so that cmake can find them easily.
All you have to do is copy this code to your cmakelists.txt prefferable at the top before calling findogre(...) also make sure you have 7z installed.

```
#----------------------------------------------------------------------
if(EXISTS ${CMAKE_SOURCE_DIR}/JSONParser.cmake)
    include(JSONParser.cmake)
    set(Downloaded_JS TRUE)
else ()
    file(DOWNLOAD "https://raw.githubusercontent.com/sbellus/json-cmake/master/JSONParser.cmake" "${CMAKE_SOURCE_DIR}/JSONParser.cmake" SHOW_PROGRESS STATUS status)
    #message(STATUS "Download status = ${status}")
    if(EXISTS ${CMAKE_SOURCE_DIR}/JSONParser.cmake)
        include(JSONParser.cmake)
        set(Downloaded_JS TRUE)
    endif()
endif()



if (WIN32)
  set (show_contents_prog type)
endif (WIN32)

if(EXISTS " ${OGRE_HOME}/current_ogre")
    file(READ "${OGRE_HOME}/current_ogre" current_version)
else()
    set(current_version 0)
endif()
set(Ogre_zip "${OGRE_HOME}/Latest_ogre.zip")
string(REPLACE "/" "\\"  Ogre_zip  ${Ogre_zip})
string(REPLACE "\\" "/"  OGRE_HOME  ${OGRE_HOME})

if(Downloaded_JS)
    file(DOWNLOAD "https://api.github.com/repos/kenkit/ogre/releases/latest" "${CMAKE_SOURCE_DIR}/bin/json_release.json" SHOW_PROGRESS STATUS status)
    file(READ "${CMAKE_SOURCE_DIR}/bin/json_release.json" JSON_OUTPUT)
    sbeParseJson(parsed_json JSON_OUTPUT)
    set(latest_version ${parsed_json.tag_name})
    set(download_url ${parsed_json.assets_0.browser_download_url}) 

    string(COMPARE EQUAL "${latest_version}" "" result)
    if(result)
        message("No network??")  
       set(latest_version ${current_version})
    endif()
    message("LATEST VERSION OF OGRE:REPO KENIT/OGRE ${latest_version} CURRENTLY INSTALLED ${current_version}")
    string(REPLACE "." "" current_version ${current_version})
    string(REPLACE "." ""  latest_version_2 ${latest_version})
  
    if((current_version  LESS  latest_version_2 ) OR (current_version LESS_EQUAL "0" ))
        message("DOWNLOADING NEW OGRE FROM KENKITS REPO:${download_url}" )
        file(DOWNLOAD "${download_url}" "${Ogre_zip}" SHOW_PROGRESS STATUS status)
       message("Removing old ogre in:${OGRE_HOME} ")
       if(EXISTS ${OGRE_HOME}/build/sdk)
            FILE(REMOVE_RECURSE ${OGRE_HOME}/build)
      endif()
       file(WRITE ${OGRE_HOME}/extract.bat "\"${7ZIP_EXECUTABLE}\" x  -y -r ${Ogre_zip} -o${OGRE_HOME}")

       list(GET status 0 status_code)
       list(GET status 1 status_string)
        if(NOT status_code EQUAL 0)
        message(FATAL_ERROR "error: downloading failed
        status_code: ${status_code}
        status_string: ${status_string}")
        else()
            message("Download succeeded")
            file(WRITE "${OGRE_HOME}/current_ogre" ${latest_version})
            execute_process(COMMAND ${OGRE_HOME}/extract.bat RESULT_VARIABLE rv)
            file(REMOVE ${OGRE_HOME}/extract.bat)
            
            message("Setting ogre_dir environment variable")
            file(WRITE ${OGRE_HOME}/env.bat "setx OGRE_DIR \"${OGRE_HOME}/build/sdk\"")
            execute_process(COMMAND "${OGRE_HOME}/env.bat"  RESULT_VARIABLE rv)  
            message("7z='${rv}'")

            file(WRITE ${OGRE_HOME}/env.bat "set OGRE_DIR =\"${OGRE_HOME}/build/sdk\"")
            execute_process(COMMAND ${OGRE_HOME}/env.bat  RESULT_VARIABLE rv)
            file(REMOVE ${OGRE_HOME}/env.bat)
            message("7z='${rv}'")  

            file(WRITE ${OGRE_HOME}/env.bat  "setx SDL2DIR \"${OGRE_HOME}/build/sdk/ogredeps/cmake\"")
            message("Setting SDL2_DIR environment variable")
            execute_process(COMMAND  ${OGRE_HOME}/env.bat RESULT_VARIABLE rv) 
            file(REMOVE ${OGRE_HOME}/env.bat) 
            message("7z='${rv}'")

            file(WRITE ${OGRE_HOME}/env.bat  "set SDL2DIR =\"${OGRE_HOME}/build/sdk\"")
            execute_process(COMMAND   ${OGRE_HOME}/env.bat  RESULT_VARIABLE rv) 
            file(REMOVE ${OGRE_HOME}/env.bat) 
            message("7z='${rv}'")

            file(WRITE ${OGRE_HOME}/env.bat "setx SDL2_DIR \"${OGRE_HOME}/build/sdk\"")
            execute_process(COMMAND  ${OGRE_HOME}/env.bat  RESULT_VARIABLE rv) 
            file(REMOVE ${OGRE_HOME}/env.bat)
            message("7z='${rv}'") 

            file(WRITE ${OGRE_HOME}/env.bat "set SDL2_DIR =\"${OGRE_HOME}/build/sdk\"")
            execute_process(COMMAND ${OGRE_HOME}/env.bat  RESULT_VARIABLE rv)
            file(REMOVE ${OGRE_HOME}/env.bat)
            message("7z='${rv}'")  
        endif()     
    else ()
        message("YOU HAVE THE LATEST OGRE INSTALLED")
    endif()
endif()


#----------------------------------------------------------------------
```
## What is not working
Only ogre 2.1 builds are failing, will work on this....

Fixed sdl2 detection in cmake

## TODO
Make current_ogre to be in OGRE_HOME So it's not deleted and will make the script work on other projects too
## Build Status
| Build | Status (github) |
|-------|-----------------|
| OGRE 2.1 MSVC | [![Build status](https://ci.appveyor.com/api/projects/status/q4q8yqy7uad0utmd?svg=true)](https://ci.appveyor.com/project/kenkit/ogre-6fnyg)
| OGRE 1.11 MSVC | [![Build status](https://ci.appveyor.com/api/projects/status/s0l07pa1uo7coda2?svg=true)](https://ci.appveyor.com/project/kenkit/ogre-mm8lb)
