# How to Build on Windows

## Important Notes
* The official binaries for Windows are built with Qt 5 and MinGW (Visual Studio builds are also available).
* The compiler toolkit must be the same that was used to build Qt.
* This page is out-of-date so try to use latest libraries and build tools.
* Commits in github repository are automatically built and tested at [AppVeyor](https://ci.appveyor.com/project/hluk/copyq)
(see latest [scripts](https://github.com/hluk/CopyQ/tree/master/utils/appveyor)
and [config file](https://github.com/hluk/CopyQ/blob/master/appveyor.yml)).

## Download and install CMake 2.8.12.2
CMake is used to generate the Makefiles.
* Download and install [CMake 2.8.12.2 Windows (Win32 Installer)](http://www.cmake.org/files/v2.8/cmake-2.8.12.2-win32-x86.exe)
* Select the option `Add CMake to the system PATH for all users` during the installation

## Download and install Qt
You have the choice between Qt 4.8.5 + MinGW 4.4, Qt 4.8.5 + Visual Studio C++ 2010 Express, Qt 5.2.1 + MinGW 4.8 or Qt 5.2.1 + Visual Studio C++ 2010 Express
### Qt 4.8.5
#### with MinGW 4.4
* Download and install [Qt libraries 4.8.5 for Windows (minGW 4.4, 317 MB)](http://download.qt-project.org/official_releases/qt/4.8/4.8.5/qt-win-opensource-4.8.5-mingw.exe).
* After the standard installation append the following line to your `PATH` environment variable:
`;c:\Qt\4.8.5\bin;c:\Qt\4.8.5\lib;c:\mingw\bin;`
* Since Qt 4.8.5 does not include the MinGW compiler it is the easiest to download and extract the old MinGW 4.4 version from http://nosymbolfound.blogspot.de/2012/12/since-until-now-qt-under-windows-is.html

#### with Visual Studio C++ 2010 Express (VC2010)
* Download and install [Qt libraries 4.8.5 for Windows (VS 2010, 235 MB)](http://download.qt-project.org/official_releases/qt/4.8/4.8.5/qt-win-opensource-4.8.5-vs2010.exe)
* Download and install [Visual Studio C++ 2010 Express](http://www.visualstudio.com/de-de/downloads/download-visual-studio-vs#DownloadFamilies_4)

### Qt 5.2.1
#### with MinGW 4.8
* Download and install Qt 5.2.1 [Qt 5.2.1 for Windows 32-bit (MinGW 4.8, OpenGL, 634 MB)](http://download.qt-project.org/official_releases/qt/5.2/5.2.1/qt-opensource-windows-x86-mingw48_opengl-5.2.1.exe)
* After the standard installation append the following line to your `PATH` environment variable:
`;C:\Qt\Qt5.2.1\Tools\mingw48_32\bin;C:\Qt\Qt5.2.1\Tools\mingw48_32\lib;C:\Qt\Qt5.2.1\5.2.1\mingw48_32\bin;`

#### with Visual Studio C++ 2010 Express (VC2010)
* Download and install [Qt 5.2.1 for Windows 32-bit (VS 2010, 518 MB)](http://download.qt-project.org/official_releases/qt/5.2/5.2.1/qt-opensource-windows-x86-msvc2010-5.2.1.exe)
* Download and install [Visual Studio C++ 2010 Express](http://www.visualstudio.com/de-de/downloads/download-visual-studio-vs#DownloadFamilies_4)

## Makefiles
### Generating Makefiles
1. clone the repository: `git clone https://github.com/hluk/CopyQ.git`
1. change into the local copy of the repository: `cd CopyQ`
1. create a new folder e.g. **build** and change into this directory: `mkdir build && cd build`
1. generate the Makefiles in the **build** directory: `cmake ..`

Depending on which Qt version and compiler you want to use you have to generate the Makefiles differently
* Qt 4.8.5 and MinGW 4.4: `cmake -G "MinGW Makefiles" ..`
* Qt 4.8.5 and VC2010: `cmake -G "Visual Studio 10" ..`
* Qt 5.2.1 and MinGW 4.8: `cmake -DWITH_QT5=TRUE -G "MinGW Makefiles" ..`
* Qt 5.2.1 and VC2010: `cmake -DWITH_QT5=TRUE -G "Visual Studio 10" ..`

If you want to build with tests (https://github.com/hluk/CopyQ/wiki/Running-Tests) and debug use the additional `cmake` switches: `-DCMAKE_BUILD_TYPE=Debug -DWITH_TESTS=TRUE`
For example Qt 5.2.1 and MinGW 4.8: `cmake -DWITH_QT5=TRUE -DCMAKE_BUILD_TYPE=Debug -DWITH_TESTS=TRUE -G "MinGW Makefiles" ..`

## Build
### with MinGW
* Execute via command line in the **build** directory: `mingw32-make`

### with VC2010
* Open `Visual Studio Command Prompt (2010)` via start menu
* Execute in the **build** directory: `msbuild copyq.sln /p:Configuration=Release`

## Additional Qt 5.2.1 dependencies
If you would like to use the CopyQ build on another Windows without Qt 5.2.1 installation you have to copy some files from Qt 5.2.1 into the CopyQ directory:

My CopyQ directory looks like this:
```
|   copyq.com
|   copyq.exe
|   icudt51.dll
|   icuin51.dll
|   icuuc51.dll
|   libEGL.dll
|   libgcc_s_dw2-1.dll
|   libGLESv2.dll
|   libstdc++-6.dll
|   libwinpthread-1.dll
|   msvcp100.dll
|   msvcr100.dll
|   Qt5Core.dll
|   Qt5Gui.dll
|   Qt5Network.dll
|   Qt5Script.dll
|   Qt5Svg.dll
|   Qt5Widgets.dll
|   
+---imageformats
|       qgif.dll
|       qico.dll
|       qjpeg.dll
|       qmng.dll
|       qsvg.dll
|       qtga.dll
|       qtiff.dll
|       
+---platforms
|       qminimal.dll
|       qoffscreen.dll
|       qwindows.dll
|       
+---plugins
|       libitemdata.dll
|       libitemencrypted.dll
|       libitemfakevim.dll
|       libitemimage.dll
|       libitemnotes.dll
|       libitemsync.dll
|       libitemtext.dll
|       libitemweb.dll
|       
\---themes
        dark.ini
        forest.ini
        paper.ini
        simple.ini
        solarized-dark.ini
        solarized-light.ini
        wine.ini
```
