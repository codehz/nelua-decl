C binding generator for [Nelua](https://nelua.io/) using [GCC Lua plugin](https://peter.colberg.org/gcc-lua).

This tool assists creating C bindings for any C library
for Nelua in a few steps, also enables the possibility to customize
each library via Lua scripts. Requires GCC to run.

**Note: You need GCC plugin mechanism to use this (only available in Linux systems).**

## Usage

Make sure you are using Lua 5.3+, GCC 9+ and have GCC plugin installed.

First clone and compile the `gcc-lua` plugin and compile it:

```bash
git clone --recurse-submodules https://github.com/edubart/nelua-decl.git
make -C gcc-lua
```

Now you can compile bindings with the following command:

```bash
gcc -S libs/lua/lua.c \
    -fplugin=./gcc-lua/gcc/gcclua.so \
    -fplugin-arg-gcclua-script=libs/lua/lua.lua \
    > libs/lua/lua.nelua
```

Command explanation:
* `-S libs/lua/lua.c` tells GCC to compile only the assembly instructions for the file, this is sufficient for parsing
* `-fplugin=./gcc-lua/gcc/gcclua.so` tells GCC to load the gcc-lua plugin before compiling
* `-fplugin-arg-gcclua-script=libs/lua/lua.lua` is the lua script loaded with configurations to generate the bindings
* `libs/lua/lua.nelua` is the output file

## Bindings example

The following libraries bindings are generated as example:

* [SDL2](https://www.libsdl.org/) - [sdl2.nelua](https://github.com/edubart/nelua-decl/blob/main/libs/sdl2/sdl2.nelua) - [sdl2_image.nelua](https://github.com/edubart/nelua-decl/blob/main/libs/sdl2/sdl2_image.nelua) - [sdl2_ttf.nelua](https://github.com/edubart/nelua-decl/blob/main/libs/sdl2/sdl2_ttf.nelua) - [sdl2_mixer.nelua](https://github.com/edubart/nelua-decl/blob/main/libs/sdl2/sdl2_mixer.nelua)
* [GLFW](https://www.glfw.org/) - [glfw.nelua](https://github.com/edubart/nelua-decl/blob/main/libs/glfw/glfw.nelua)
* [minilua](https://github.com/edubart/minilua) (Lua 5.4) - [lua.nelua](https://github.com/edubart/nelua-decl/blob/main/libs/minilua/minilua.nelua)
* [minicoro](https://github.com/edubart/minicoro) - [minicoro.nelua](https://github.com/edubart/nelua-decl/blob/main/libs/minicoro/minicoro.nelua)
* [miniaudio](https://miniaud.io/) - [miniaudio.nelua](https://github.com/edubart/nelua-decl/blob/main/libs/miniaudio/miniaudio.nelua)
* [miniphysfs](https://github.com/edubart/miniphysfs) (PhysFS 3) - [miniphysfs.nelua](https://github.com/edubart/nelua-decl/blob/main/libs/miniphysfs/miniphysfs.nelua)
* [STB](https://github.com/nothings/stb) - [stb_image.nelua](https://github.com/edubart/nelua-decl/blob/main/libs/stb/stb_image.nelua) - [stb_image_write.nelua](https://github.com/edubart/nelua-decl/blob/main/libs/stb/stb_image_write.nelua) - [stb_truetype.nelua](https://github.com/edubart/nelua-decl/blob/main/libs/stb/stb_truetype.nelua) - [stb_vorbis.nelua](https://github.com/edubart/nelua-decl/blob/main/libs/stb/stb_vorbis.nelua)
* [Sokol](https://floooh.github.io/sokol-html5/index.html) - [sokol_gfx.nelua](https://github.com/edubart/nelua-decl/blob/main/libs/sokol/sokol_gfx.nelua) - [sokol_app.nelua](https://github.com/edubart/nelua-decl/blob/main/libs/sokol/sokol_app.nelua) - [sokol_glue.nelua](https://github.com/edubart/nelua-decl/blob/main/libs/sokol/sokol_glue.nelua) - [sokol_audio.nelua](https://github.com/edubart/nelua-decl/blob/main/libs/sokol/sokol_audio.nelua) - [sokol_time.nelua](https://github.com/edubart/nelua-decl/blob/main/libs/sokol/sokol_time.nelua)
* [Blend2D](https://blend2d.com/) - [blend2d.nelua](https://github.com/edubart/nelua-decl/blob/main/libs/blend2d/blend2d.nelua)
* [Chipmunk](https://chipmunk-physics.net/) - [chipmunk.nelua](https://github.com/edubart/nelua-decl/blob/main/libs/chipmunk/chipmunk.nelua)
* [Raylib](https://www.raylib.com/) - [raylib.nelua](https://github.com/edubart/nelua-decl/blob/main/libs/raylib/raylib.nelua)
* [pthread](https://computing.llnl.gov/tutorials/pthreads/) - [pthread.nelua](https://github.com/edubart/nelua-decl/blob/main/libs/pthread/pthread.nelua)
* [libuv](https://libuv.org/) - [uv.nelua](https://github.com/edubart/nelua-decl/blob/main/libs/uv/uv.nelua)

## How to generate bindings

Some libraries such as SDL2, GLFW and Lua
must be installed on your system.
Other libraries like Sokol and miniaudio must be
downloaded with `make download`.

To generate bindings for them simply do `make sdl2`
and `libs/sdl2/sdl2.nelua` will be generated for example.

## Tests

Some tests are present as examples on how to use
the libraries, you can run the as long you
have the library headers on your system. Look
for example the `libs/sdl2/sdl2-test.nelua` file.

## Caveats

Currently GCC plugin does not work on Windows, thus you have to generate
bindings from a Linux machine.

## TODO

* Support C bit fields
* Support C unnamed fields
* Support C math complex type
* Better handle nelua reserved keyword names
