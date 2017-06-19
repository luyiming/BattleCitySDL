# BattleCitySDL

* License: [MIT](https://mit-license.org/)
* Windows Version Release
* Linux

# Project Info

Remake of Battle City (a.k.a. Tank1990). Written in C++11 and OpenGL using SDL2.0.

# Screenshots

# Building Source

## Prerequisites

The following libraries are required to build:

- SDL2.0
- SDL_image
- SDL_mixer
- OpenGL

Please make sure that all dependancies are installed and linked properly to your PATH before building.

### Windows
Libraries files are already put in the `lib/` folder. You can replace them with the newest library files from SDL2.0 if you like.

### Debian/Ubuntu
Simply run the following commands:
```
sudo apt-get install libsdl2-dev libsdl2_image-dev libsdl2_mixer-dev
sudo apt-get install mesa_dev
```

## Build Instructions

### Linux
```bash
mkdir build
cd build
cmake .. # You can switch to other build systems, see cmake manual
make     # the default build system on Linux is "Unix Makefiles"
./BattleCitySDL # run the game
```
### Windows
```bash
mkdir build
cd build
cmake .. # the default build system on Windows is Visual Studio
```
Open `BattleCitySDL.sln` file in `build/` folder. You can then build and run the project in Visual Studio. The executable file should be in build/Debug.
