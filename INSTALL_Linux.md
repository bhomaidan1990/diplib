# Building the *DIPlib* project on Linux

Compiling *DIPlib* requires a few programs that often are not present on a bare Linux
installation. It depends on your distribution how you can obtain these. Below we give
instructions for Ubuntu (TODO: add instructions for other distros).

See [`INSTALL.md`](INSTALL.md) for general concepts and additional information
on the compilation options.

Everything here requires a shell (such as Bash), which typically runs inside a terminal
window. If you're using Linux we'll assume you know where to find this.

## Installing build tools and dependencies on Ubuntu

Type the following command in your shell:
```bash
sudo apt install build-essential cmake git
```
This will install all the required tools. Depending on your version of Ubuntu, you'll
have a compiler that is too old to correctly build *DIPlib*. To see which version
of GCC you have, type:
```bash
g++ --version
```
You want to see at least version 5.4, but later versions would be better. If your
version of GCC is older, you will need to manually install a newer version.

If you want to compile *DIPviewer*, you need to install `freeglut` as well:
```bash
sudo apt install freeglut3-dev
```

To compile the documentation yourself (note that the compiled documentation can be found
online), you need *Doxygen*:
```bash
sudo apt install doxygen
```

Finally, even if the `python3` program is already installed, you might need to
install the *Python 3* header files and static library:
```bash
sudo apt install python3-dev
```

## Cloning the repository

Next, get the source repository from *GitHub*:
```bash
mkdir ~/src
cd ~/src
git clone https://github.com/DIPlib/diplib.git
```
This creates a directory `src/diplib` in your home directory.

## Building

As explained in the [`INSTALL.md`](INSTALL.md) file, type
```bash
mkdir ~/src/diplib/target
cd ~/src/diplib/target
cmake ..
make -j check
make -j install
```

This will install to `/usr/local`. If you prefer to install elsewhere, change the
`cmake` line with the following:
```bash
cmake .. -DCMAKE_INSTALL_PREFIX=$HOME/dip
```
This will install *DIPlib*, *DIPviewer*, *DIPjavaio*, *DIPimage* and the documentation
under the `dip` directory in your home directory

*PyDIP* is installed separately through `pip`. Once the `install` target has finished building
and installing, run
```bash
make pip_install
```

We recommend you additionally specify the `-DCMAKE_CXX_FLAGS="-march=native"`
option to `cmake`. This will enable additional optimizations that are specific
to your computer. Note that the resulting binaries will likely be slower on another
computer, and possibly not work at all.

You can also do
```bash
make apidoc
```
if you want to compile the *DIPlib* documentation.

## Enabling Bio-Formats

First, make sure you have the Java 8 SDK (JDK 8) installed, you can obtain it from the
[Oracle website](http://www.oracle.com/technetwork/java/javase/downloads/index.html). Next, download
`bioformats_package.jar` from the [Bio-Formats website](https://www.openmicroscopy.org/bio-formats/).
You need to add the location of this file to the `cmake` command line using the `-DBIOFORMATS_JAR=<path>`
flag.

When running *CMake* with the proper JDK installed, the *DIPjavaio* module becomes available.

## Using *DIPimage*

Once the `install` target has finished building and installing the toolbox, start
*MATLAB*. Type the following command:
```matlab
addpath('/Users/<name>/dip/share/DIPimage')
```
This will make the toolbox available (replace `/Users/<name>/dip` with the
actual path you installed to).

To get started using *DIPimage*, read the *DIPimage User Manual*, and look through
the help, starting at
```matlab
help DIPimage
```
Or start the GUI:
```matlab
dipimage
```

## Using *PyDIP*

Once the `pip_install` target has finished installing, start *Python*.
The following command will import the *PyDIP* package as `dip`, which is shorter to
type and mimics the namespace used in the C++ library:
```python
import diplib as dip
```

To get started using *PyDIP*, look through the help, starting at
```python
help(dip)
```
