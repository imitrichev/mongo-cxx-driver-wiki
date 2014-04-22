> New in version 2.5.3: The procedure for compiling the C++ driver has changed, beginning with version 2.5.3.

The current C++ driver is still compatible with earlier versions of MongoDB. For example, if you run MongoDB 2.4, you can use the C++ driver compiled from MongoDB 2.5.3+. If, however, you need instructions for compiling the driver from the pre-2.5.3 code, see Download and Compile C++ Driver Before Version 2.5.3.

### Prerequisites
 - [Boost](http://www.boost.org/) >= 1.49
 - [Python](https://www.python.org/)
 - [Scons](http://www.scons.org/)

**IMPORTANT**
The C++ driver requires Boost version 1.49 or greater. Earlier versions of Boost may work, but the behavior of the driver with those versions is untested.

### Clone the Driver Source Code

```sh
git clone git@github.com:mongodb/mongo-cxx-driver.git
```

### Compile the Driver

From the directory where you cloned the code, compile the C++ driver by running the scons command. Use the scons options described in this section.

To see the list of all SCons options, run:

`scons --help`

**NOTE**
#### SCons Options when Compiling the C++ Driver
Select options as appropriate for your environment.

```sh
--prefix=<path> The directory prefix for the installation directory. Set <path> to the directory where you want the build artifacts (headers and library files) installed. For example, you might set <path> to /opt/local, /usr/local, or $HOME/mongo-client-install.
--full Enables the “full” installation, directing SCons to install the driver headers and libraries to the prefix directory.
--ssl Enables SSL support. You will need a compatible version of the SSL libraries available.
--use-sasl-client Enables SASL, which MongoDB uses for the Kerberos authentication available on MongoDB Enterprise. You will need a compatible version of the SASL implementation libraries available.
--sharedclient Builds a shared library version of the client driver alongside the static library. If applicable for your application, prefer using the shared client.
--use-system-boost This is strongly recommended. This builds against the system version of Boost rather than the MongoDB vendor copy. If your Boost libraries are not in a standard search path for your toolchain, include the --extrapath option, described next.
```

**NOTE**
While the C++ driver should in general be built against the system version of Boost, if you are building the MongoDB servers and tools from source, it is strongly recommended that you use the embedded version of Boost. This means that your build line for the driver and server will differ with respect to the --use-system-boost flag.

```sh
--extrapath=<path-to-boost>. Specifies the path to your Boost libraries if they are not in a standard search path for your toolchain.
install-mongoclient. This is the build target.
--dbg=[on|off]. Enables runtime debugging checks. Defaults to off. Specifying --dbg=on implies --opt=off unless explicitly overridden with --opt=on.
--opt=[on|off]. Enables compile-time optimization. Defaults to on. Can be freely mixed with the values for the --dbg flag.
--cc. The compiler to use for C. Use the following syntax:
--cc=<path-to-c-compiler>
--cxx. The compiler to use for C++. Use the following syntax:
--cxx=<path-to-c++-compiler>
--dynamic-windows. By default, on Windows, compilation uses /MT. Use this flag to compile with /MD. Note that /MD is required to build the shared client on Windows. Also note that your application compiler flags must match.
```

If you build with --dbg=on, /MTd or /MDd will be used in place of /MT or /MD, respectively.
Windows Considerations
When building on Windows, use of the SCons --dynamic-windows option can result in an error unless all libraries and sources for the application use the same runtime library. This option builds the driver to link against the dynamic windows libraries instead of the static windows runtime libraries. If the Boost library being linked against is expecting an /MT build (static libraries), this can result in an error similar to the following:
error LNK2005: ___ already defined in msvcprt.lib(MSVCP100.dll) libboost_thread-vc100-mt-1_42.lib(thread.obj)
You may want to define _CRT_SECURE_NO_WARNINGS to avoid warnings on use of strncpy and such by the MongoDB client code.
Include the WinSock library in your application: Linker ‣ Input ‣ Additional Dependencies. Add ws2_32.lib.

### Example C++ Driver Compilations

The following are examples of building the C++ driver.

The following example installs the new driver to $HOME/mongo-client-install, enables the “full” installation, and builds against the system version of Boost:

```sh
scons --prefix=$HOME/mongo-client-install --full --use-system-boost install-mongoclient
To enable SSL, add the --ssl option:
```

```sh
scons --prefix=$HOME/mongo-client-install --full --use-system-boost --ssl install-mongoclient
To enable SASL support for use with Kerberos authentication on MongoDB Enterprise, add the --use-sasl-client option:
```

```sh
scons --prefix=$HOME/mongo-client-install --full --use-system-boost --use-sasl-client install-mongoclient
```

To build a shared library version of the driver, along with the normal static library, use the --sharedclient option:

```sh
scons --prefix=$HOME/mongo-client-install --full --use-system-boost --sharedclient install-mongoclient
To use a custom version of boost installed to /dev/boost, use the --extrapath=<path-to-boost> option:
```

```sh
scons --prefix=$HOME/mongo-client-install --full --use-system-boost --extrapath=/dev/boost install-mongoclient
```

To build a version of the library with debugging enabled, use --dbg=on. This turns off optimization, which is on by default. To enable both debugging and optimization, pass --dbg=on --opt=on:

```sh
scons --prefix=$HOME/mongo-client-install --full --use-system-boost --dbg=on --opt=on install-mongoclient
```

To override the default compiler to a newer GCC installed in /opt/local/gcc-4.8, use the --cc and --cxx options:

```sh
scons --prefix=$HOME/mongo-client-install --full --use-system-boost --cc=/opt/local/gcc-4.8/bin/gcc --cxx=/opt/local/gcc-4.8/bin/g++ install-mongoclient
```

To build as a DLL on Windows:

New in version 2.5.5.

```sh
scons <--64 or --32> --mute --sharedclient --dynamic-windows --release --full --prefix=<install-path> --use-system-boost --cpppath=<path-to-boost-headers> --libpath=<path-to-boost-libs> --variant-dir=<path-to-a-variant-directory> install-mongoclient
```

The following example issues the build command in a PowerShell:
```sh
scons `
    --64 `
    --mute `
    --sharedclient `
    --dynamic-windows `
    --release `
    --full `
    --prefix=%HOME%\mongo-client-install `
    --use-system-boost `
    --cpppath=C:\local\boost_1_55_0\include `
    --libpath=C:\local\boost_1_55_0\lib64-msvc-12.0 `
    --variant-dir=%HOME%\mongo-client-build `
    install-mongoclient
```

**NOTE**
You must configure Visual Studio for release builds when building your application with the C++ driver DLL.
Disable STL iterator debugging to ensure compatibility with the STL binary.
You must use Boost v1.49 or greater.