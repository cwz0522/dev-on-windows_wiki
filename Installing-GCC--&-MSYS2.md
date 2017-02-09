GCC is a very good compiler collection, and is fully free (in speech and beer). There are however a ton of Windows distributions spread over the internet, but only some are of high quality. There's a lot of choices to be made as well, so I've made all those for you so you don't have to worry, and get good defaults.

We will use the MinGW-w64 port packaged with MSYS2. This also allows us to use *nix toolchains to build other libraries, as well as use the precompiled libraries that MSYS2 provides. Note that MSYS2 provides MinGW-w64 compilers. Binaries with these compilers will be standalone, and do __not__ require a `cygwin.dll` or similar file.

### I assume your _development_ machine is 64-bit, and you want your compiler to _target_ 64-bit windows by default. We will install both a 32-bit and a 64-bit _target_ compiler toolchain, regardless.

__If you have a 32-bit _development_ machine, change every occurrence of `C:\dev\msys64` with `C:\dev\msys32` below.__ However, it's `<current year>`, get a 64-bit machine.

1. Download [msys2-x86_64-latest.exe](http://repo.msys2.org/distrib/msys2-x86_64-latest.exe) and run it. If your development machine is 32-bit, download [msys2-i686-latest.exe](http://repo.msys2.org/distrib/msys2-i686-latest.exe) instead. Make sure to set the install directory to `C:\dev\msys64` (`C:\dev\msys32` for 32-bit). Choose to run MSYS2 right now.

2. In the MSYS2 shell, execute the following. Hint: if you right click the title bar, go to Options -> Keys and tick "Ctrl+Shift+letter shortcuts" you can use Ctrl+Shift+V to paste in the MSYS shell.

    ```
   pacman -Syuu
    ```

3. __Close the MSYS2 shell once you're asked to.__ There are now 3 MSYS subsystems installed: MSYS2, MinGW32 and MinGW64. They can respectively be launched from `C:\dev\msys64\msys2.exe`, `C:\dev\msys64\mingw32.exe` and `C:\dev\msys64\mingw64.exe`. _If the installer created any shortcuts to open shells for these subsystems, you can update them to these locations to get pretty icons._ Each subsystem provides an environment to build Windows applications. The MSYS2 environment is for building POSIX compliant software on Windows using an emulation layer. The MinGW32/64 subsystems are for building native Windows applications using a linux toolchain (gcc, bash, etc), targetting respectively 32 and 64 bit Windows. We will install our `PATH` such that these tools can be called from regular cmd.exe as well, and we need only use the MinGW subsystem to install/update MSYS2 packages or if our build setup requires a *nix shell. Hint: after starting up MSYS2, the prompt will say which version you launched.

4. Reopen MSYS2 (doesn't matter which version, since we're merely installing packages). __Repeatedly__ run the following command until it says there are no further updates. You might have to restart your shell again.

   ```
   pacman -Syuu
   ```

5. Now that MSYS2 is fully up-to-date we will install GCC and common build tools. When you are queried to select packages and confirm the installation just press enter:

   ```
   pacman -S --needed base-devel mingw-w64-i686-toolchain mingw-w64-x86_64-toolchain \
                       git subversion mercurial \
                       mingw-w64-i686-cmake mingw-w64-x86_64-cmake
   ```

6. Add `C:\dev\msys64\mingw64\bin` and `C:\dev\msys64\mingw32\bin`, __in that order__, to your `PATH`. Note that MSYS2 also puts a lot of other tools in this directory, most notably Python. So put these entries below any other tools you might have installed in your PATH.

Done. Now you can use `gcc`, `g++`, etc to get your 64-bit targeting compiler from your regular command line. To make 32-bit binaries, use `i686-w64-mingw32-g++` and co.

To be safe and reproducible, MSYS2 by default disables inheriting your `PATH` settings in their environments. You can toggle this option per environment by looking in the respective `.ini` file in `C:\dev\msys64` for `MSYS2_PATH_TYPE=inherit`. My recommendation is to inherit the path for the MinGW32/64 environments, but keeping the MSYS environment pure.

---

### The instructions below are an example of installing a library, this part is not required.

First and foremost I suggest checking the package manager of MSYS2. It has a lot of pre-built library packages. You can search the package repository using `pacman -Ss your_library`, for example:

    $ pacman -Ss boost
    mingw32/mingw-w64-i686-boost 1.60.0-2
        Free peer-reviewed portable C++ source libraries (mingw-w64)
    mingw64/mingw-w64-x86_64-boost 1.60.0-2
        Free peer-reviewed portable C++ source libraries (mingw-w64)

If the package name starts with mingw, it's a library. Install it using `pacman -Sy package_name`, e.g.:

    $ pacman -Sy mingw-w64-i686-boost mingw-w64-x86_64-boost
    :: Synchronizing package databases...
     mingw32 is up to date
     mingw64 is up to date
     msys is up to date
    resolving dependencies...
    looking for conflicting packages...

    Packages (2) mingw-w64-i686-boost-1.60.0-2  mingw-w64-x86_64-boost-1.60.0-2

    Total Installed Size:  546.33 MiB

    :: Proceed with installation? [Y/n] y
    (2/2) checking keys in keyring
    (2/2) checking package integrity
    (2/2) loading package files
    (2/2) checking for file conflicts
    (2/2) checking available disk space
    (1/2) installing mingw-w64-i686-boost
    (2/2) installing mingw-w64-x86_64-boost

Sadly there is no wildcard, but you can use <code>pacman -Sy \`pacman -Ssq boost\`</code> to install everything returned by a search.

If your library is not in the package manager you must compile it yourself. As an example, we'll try and build the 64-bit zlib library (this is an excercise - zlib is installed already by default):

1. Open `mingw64.exe` (and if not already, cd to `~`).

2. Download and unpack zlib:

   ```
   wget http://zlib.net/zlib-1.2.8.tar.gz
   tar xf zlib-1.2.8.tar.gz
   cd zlib-1.2.8
   ```

3. Configure, compile and install:

   ```
   make -f win32/Makefile.gcc install BINARY_PATH=/mingw64/bin \
   INCLUDE_PATH=/mingw64/include LIBRARY_PATH=/mingw64/lib
   cd ..
   ```
