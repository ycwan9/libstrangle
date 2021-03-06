# libstrangle
Frame rate limiter for Linux.
## Usage
Cap the FPS (frames per second) of a chosen game by including libstrangle.so in LD_PRELOAD. The environment variable FPS needs to be set first.
Example:
```
export FPS=60
LD_PRELOAD="libstrangle.so:${LD_PRELOAD}" /path/to/game
```
The included script strangle.sh, which installs into your PATH as just "strangle", can be used to simplify this.
Examples:
```
strangle 60 /path/to/game
```
### Vsync
Vertical sync can be controlled by setting the `VSYNC` environment variable.

**OpenGL**
* -1 - Adaptive sync (unconfirmed if this actually works)
* 0 - Force off
* 1 - Force on
* n - Sync to refresh rate / n.

**Vulkan**
* 0 - Force off
* 1 - Mailbox mode. Vsync with uncapped framerate.
* 2 - Traditional vsync with framerate capped to refresh rate.
* 3 - Adaptive vsync with tearing at low framerates.

Examples:
```
VSYNC=2 strangle /path/to/game
VSYNC=1 strangle 40 /path/to/game
```
### Steam
You can use this with Steam by right-clicking on a game in your library and selecting Properties and then SET LAUNCH OPTIONS... under the General tab. In the input box type:
`strangle <somenumber> %command%`
### Experimental stuff
![Mip map lod bias example](screenshots/picmip_quake.png)*vkQuake with PICMIP=1337*

You can adjust the mipmap lod bias in both opengl and vulkan with the environment variable `PICMIP`. A higher value means blurrier textures. A negative value could make textures crisper.
## Building
If you installed a version before 2016-05-17 you should manually remove the files /usr/bin/strangle, /usr/lib/i386-linux-gnu/libstrangle.so and /usr/lib/x86_64-linux-gnu/libstrangle.so - The paths have changed.
Typically you'll use these commands to build the program
```
make
sudo make install
```

**Debian**, **Ubuntu** and derivates may need some or all of these packages:
```
libc6-dev-i386
gcc-multilib
ia32-libs
```

**OpenSUSE** needs these packages:
```
glibc-devel-32bit
gcc
gcc-32bit
```
## Compatibility
As of 2017-02-07 it seems to work with most games, including WINE.
## Errata
Might crash if used together with other libs that hijack dlsym, such as Steam Overlay. It seems to work with Steam Overlay when placed at the end of LD_PRELOAD for some reason.
