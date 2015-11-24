Not all these recommendations may be needed for you, but if the software is relevant to your field, you simply should have them installed.

### [7-zip](http://www.7-zip.org/)

Free general-purpose archiving software that can handle a lot of formats.

### [UPX](http://upx.sourceforge.net/)

UPX is a free, portable, extendable, high-performance executable packer for several executable formats.

### [Dependency Walker](http://www.dependencywalker.com/)

Dependency Walker is an incredibly useful tool that allows you to open an executable or DLL and see exactly what DLLs it depends on, what functions they expose and which are used.

### [Path Editor](http://path-editor.software.informer.com/)

This rather obscure tool is great to quickly edit your PATH in a visual way.

### [Tuxproject.de's Vim builds](https://tuxproject.de/projects/vim/)

Proper Vim builds for Windows.

### [Everything](http://www.voidtools.com/)

A _really_ fast file __name__ (not contents) searcher for Windows. Will locate a file anywhere on your computer in the blink of an eye.

### [The Silver Searcher](https://github.com/ggreer/the_silver_searcher)

Extremely fast replacement for Ack (crossplatform - not Windows only), with good Vim integration. Does not provide Windoows builds but if you have [a MSYS2 GCC toolchain](https://github.com/orlp/dev-on-windows/wiki/Installing-GCC) installed it's really easy to compile. Open a MSYS2 MinGW-w64 Win32 shell and type:

    $ pacman -S mingw-w64-i686-pcre mingw-w64-i686-xz
    $ git clone https://github.com/ggreer/the_silver_searcher.git
    $ cd the_silver_searcher
    $ ./build.sh PCRE_CFLAGS=-DPCRE_STATIC LDFLAGS=-static