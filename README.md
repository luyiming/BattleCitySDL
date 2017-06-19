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

### Debian/Ubuntu
Simply run the following commands:
```
sudo apt-get install libsdl2-dev
```

### Windows

## Build Instructions
```
mkdir build
cd build
cmake .. -G "Unix Makefile"
make
```
