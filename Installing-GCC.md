GCC is a very good compiler collection for Windows, and is fully free (in speech and beer). There are however a ton of distributions spread over the internet, but only some are of high quality. There's a lot of choices to be made as well, so I've made all those for you so you don't have to worry, and get good defaults.

I assume your machine is 64-bit, and you want your compiler to target 64-bit windows by default. We will install both a 32-bit and a 64-bit compiler.

1. Download [x86_64-4.9.2-release-posix-seh-rt_v4-rev2.7z](http://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win64/Personal%20Builds/mingw-builds/4.9.2/threads-posix/seh/x86_64-4.9.2-release-posix-seh-rt_v4-rev2.7z/download). Inside this archive is a folder `mingw64`. Extract this folder as `C:\dev\mingw64`.

2. Download [i686-4.9.2-release-posix-sjlj-rt_v4-rev2.7z](http://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win32/Personal%20Builds/mingw-builds/4.9.2/threads-posix/sjlj/i686-4.9.2-release-posix-sjlj-rt_v4-rev2.7z/download). Inside this archive is a folder `mingw32`. Extract this folder as `C:\dev\mingw32`.

3. Add `C:\dev\mingw64\bin` and `C:\dev\mingw32\bin`, in that order, to your `PATH`.

Done. Now you can use `gcc`, `g++`, etc to get your 64-bit targetting compiler. To make 32-bit binaries, use `i686-w64-mingw32-g++` and co.

---

You also probably want to set up MSYS to make compiling of linux-based autoconf libraries easier.

1. Download [msys2-base-x86_64-20150202.tar.xz](http://sourceforge.net/projects/msys2/files/Base/x86_64/msys2-base-x86_64-20150202.tar.xz/download). Inside this archive is a folder `msys64`. Extract it to `C:\dev\msys64`.

2. Delete folders `C:\dev\msys64\mingw32` and `C:\dev\msys64\mingw64`.

3. Open command prompt as administrator and run:

   ```
   mklink /d c:\dev\msys64\mingw64 c:\dev\mingw64
   mklink /d c:\dev\msys64\mingw32 c:\dev\mingw32
   ```

4. Open `mingw64_shell.bat`, close it (this must be done the first time), reopen and run:

   ```
   pacman --needed -Sy bash pacman pacman-mirrors msys2-runtime
   ```

5. Close again the shell and reopen it. Then run for a full system upgrade:

   ```
   pacman -Su
   ```

6. Install common tools to build:

   ```
   pacman -S patch m4 wget tar lzip diffutils make
   ```

Done. Whenever you want to build a library for the 64-bit compiler run `mingw64_shell.bat`. For 32-bit run `mingw32_shell.bat`.

### Warning: do not install any MinGW packages through `pacman`.

We use our own toolchain, not the one MSYS2 usually uses. This means that we can not use the MinGW packages in `pacman`.

---

As an example, and to test if our MSYS2 system is properly set up, we'll try and build the 64-bit zlib library:

1. Open `mingw64_shell.bat` (and if not already, cd to `~`).

2. Download and unpack zlib:

   ```
   $ wget http://zlib.net/zlib-1.2.8.tar.gz
   $ tar xf zlib-1.2.8.tar.gz
   $ cd zlib-1.2.8
   ```

3. Configure, compile and install:

   ```
   $ make -f win32/Makefile.gcc install \
   BINARY_PATH=/mingw64/bin INCLUDE_PATH=/mingw64/include LIBRARY_PATH=/mingw64/lib
   $ cd ..
   ```
