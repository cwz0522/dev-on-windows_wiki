GCC is a very good compiler collection, and is fully free (in speech and beer). There are however a ton of Windows distributions spread over the internet, but only some are of high quality. There's a lot of choices to be made as well, so I've made all those for you so you don't have to worry, and get good defaults.

We will use the MinGW-w64 port packaged with MSYS2. This also allows us to use *nix toolchains to build other libraries, as well as use the precompiled libraries that MSYS2 provides. Note that MSYS2 provides MinGW-w64 compilers. Binaries with these compilers will be standalone, and do __not__ require a `cygwin.dll` or similar file.

I assume your machine is 64-bit, and you want your compiler to target 64-bit windows by default. We will install both a 32-bit and a 64-bit compiler.

1. Download [msys2-x86_64-latest.exe](http://repo.msys2.org/distrib/msys2-x86_64-latest.exe) and run it. Make sure to set the install directory to `C:\dev\msys64`. Choose to run MSYS2 right now.

2. In the MSYS2 shell, execute:

    ```
    $ update-core
    ```

3. __Close the MSYS2 shell.__ Reopen it either from your start menu (MSYS2/MinGW-w64 Win64 Shell), or from `C:\dev\msys64\mingw64_shell.bat`. Hint: after starting up MSYS2, the prompt will say which version you launched. Generally you want the MINGW64 one.

4. Now we will update pre-installed MSYS2 packages and install GCC and common build tools. When you are queried to select packages and confirm the installation just press enter:

   ```
   $ pacman -Su base-devel mingw-w64-i686-toolchain mingw-w64-x86_64-toolchain
   $ pacman -S mingw-w64-i686-cmake mingw-w64-x86_64-cmake
   ```

5. Add `C:\dev\msys64\mingw64\bin` and `C:\dev\msys64\mingw32\bin`, __in that order__, to your `PATH`. Note that MSYS2 also puts a lot of other tools in this directory, most notably Python. So put these entries below any other tools you might have installed in your PATH.

Done. Now you can use `gcc`, `g++`, etc to get your 64-bit targeting compiler from your regular command line. To make 32-bit binaries, use `i686-w64-mingw32-g++` and co.

---

When you want to use MSYS2 to build/install *nix _libraries_, you have to run through the MSYS2 shell. When building libraries for the 64-bit compiler run `C:\dev\msys64\mingw64_shell.bat`. For 32-bit run `C:\dev\msys64\mingw32_shell.bat`.

As an example, we'll try and build the 64-bit zlib library:

1. Open `mingw64_shell.bat` (and if not already, cd to `~`).

2. Download and unpack zlib:

   ```
   $ wget http://zlib.net/zlib-1.2.8.tar.gz
   $ tar xf zlib-1.2.8.tar.gz
   $ cd zlib-1.2.8
   ```

3. Configure, compile and install:

   ```
   $ make -f win32/Makefile.gcc install BINARY_PATH=/mingw64/bin \
   INCLUDE_PATH=/mingw64/include LIBRARY_PATH=/mingw64/lib
   $ cd ..
   ```

We can also use MSYS2's pre-built libraries for MinGW-w64. For example:

```
$ pacman -S mingw-w64-i686-libpng mingw-w64-x86_64-libpng
```