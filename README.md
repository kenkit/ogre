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
## What is not working
Add release builds.Current builds are in debug mode

Fixed sdl2 detection in cmake
## Build Status
| Build | Status (github) |
|-------|-----------------|
| OGRE 2.1 MSVC | [![Build status](https://ci.appveyor.com/api/projects/status/q4q8yqy7uad0utmd?svg=true)](https://ci.appveyor.com/project/kenkit/ogre-6fnyg)
| OGRE 1.11 MSVC | [![Build status](https://ci.appveyor.com/api/projects/status/s0l07pa1uo7coda2?svg=true)](https://ci.appveyor.com/project/kenkit/ogre-mm8lb)
