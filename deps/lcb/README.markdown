# Couchbase C Client

[![Build Status](https://travis-ci.org/couchbase/libcouchbase.png?branch=master)](https://travis-ci.org/couchbase/libcouchbase)

This is the C client library for [Couchbase](http://www.couchbase.com)
It communicates with the cluster and speaks the relevant protocols
necessary to connect to the cluster and execute data operations.

## Features

* Can function as either a synchronous or asynchronous library
* Callback Oriented
* Can integrate with most other asynchronous environments. You can write your
  code to integrate it into your environment. Currently support exists for
    * [libuv](http://github.com/joyent/libuv) (Windows and POSIX)
    * [libev](http://software.schmorp.de/pkg/libev.html) (POSIX)
    * [libevent](http://libevent.org/) (POSIX)
    * `select` (Windows and POSIX)
    * IOCP (Windows Only)
* Support for operation batching
* ANSI C ("_C89_")
* Cross Platform - Tested on Linux, OS X, and Windows.

## Building

Before you build from this repository, please check the [Couchbase C
Portal](http://couchbase.com/communities/c) to see if there is a binary
or release tarball available for your needs. Since the code here is
not part of an official release it has therefore not gone through our
release testing process.

For building you have two options; the first is via GNU autotools and
the second is via CMake. Autotools provides more packaging flexibility
while CMake integrates better into your normal (C/C++) development
environment. CMake is also the only way to build the library on Windows.

### Dependencies

By default the library depends on:

* _libevent_ (or _libev_) for the primary I/O backend.
* _openssl_ for SSL transport.

On Unix-like systems these dependencies are checked for by default
while on Windows they are not checked by default.

You may compile the library without any external dependencies by passing
`--disable-plugins` to disable dependencies on _libevent_ and/or _libev_
and `--enable-ssl=no` to disable SSL support.

If you are building libcouchbase as a depdency for an application which
contains its own event loop implementation then you may specify the
`--disable-plugins` option to the configure script.

Additionally, in order to run the tests you will need to have java
installed.  The tests make use of a mock server written in Java.

The binary command line tools (i.e. `cbc`) and tests require a C++
compiler. The core library requires only C.

#### OpenSSL on OS X
Note that on recent versions of OS X, the bundled version of OpenSSL
is considered deprecated and should not be used. Rather, install a
different version of OpenSSL via homebrew, and direct the configure
script to look in that location via e.g.

```
mnunberg@mbp15 ~ $ brew list openssl
/usr/local/Cellar/openssl/1.0.1g/bin/openssl
# Install root is /usr/local/Cellar/openssl/1.0.1g
# ...
./configure --with-openssl=/usr/local/Cellar/openssl/1.0.1g
```

### Building with autotools

In order to build with autotools you need to generate the `configure` script
first. This requires `autoconf`, `automake`, `libtool` and friends.

```shell
$ ./config/autorun.sh
$ ./configure
$ make
$ make check
$ make install
```

You may run `./configure --help` to see a list of build options

### Building with CMake (*nix)

Provided is a convenience script called `cmake/configure`. It is a Perl
script and functions like a normal `autotools` script.

```shell
$ mkdir lcb-build # sibling of the git tree
$ cd lcb-build
$ ../libcouchbase/cmake/configure
$ make
$ ctest
```

### Building with CMake (Windows)

Spin up your visual studio shell and run cmake from there. It is best
practice that you make an out-of-tree build; thus like so:

Assuming Visual Studio 2010

```
C:\> git clone git://github.com/couchbase/libcouchbase.git
C:\> mkdir lcb-build
C:\> cd lcb-build
C:\> cmake -G "Visual Studio 10" ..\libcouchbase
C:\> msbuild /M libcouchbase.sln
```

This will generate and build a Visual Studio `.sln` file.

Windows builds are known to work on Visual Studio versions 2008, 2010 and
2012.

If you wish to link against OpenSSL, you should set the value of
`OPENSSL_ROOT_DIR` to the location of the installation path, as described
[here](https://github.com/Kitware/CMake/blob/master/Modules/FindOpenSSL.cmake)

## Bugs, Support, Issues
You may report issues in the library in our issue tracked at
<http://couchbase.com/issues>. Sign up for an account and file an issue
against the _Couchbase C Client Library_ project.

The developers of the library hang out in IRC on `#libcouchbase` on
irc.freenode.net.


## Examples

* The `examples` directory
* Client libraries wrapping this library
    * [node.js](http://github.com/couchbase/couchnode)
    * [Python](http://github.com/couchbase/couchbase-python-client)
    * [Ruby](http://github.com/couchbase/couchbase-ruby-client)

## Documentation

API documentation may be generated by running `doxygen` within the source root
directory. When this is done, you should have a `doc/html/index.html` page which
may be viewed.

Doxygen may be downloaded from the
[doxygen downloads page](http://www.stack.nl/~dimitri/doxygen/download.html). Note
however that most Linux distributions as well as Homebrew contain Doxygen in their
repositories.

```
$ doxygen
$ xdg-open doc/html/index.html # Linux
$ open doc/html/index.html # OS X
```

You may also generate documentation using the `doc/Makefile` which dynamically
inserts version information

```
$ make -f doc/Makefile public # for public documentation
$ make -f doc/Makefile internal # for internal documentation
```

The generated documentation will be in the `doc/public/html` directory for
public documentation, and in the `doc/internal/html` directory for internal
documentation.

## Contributors

See the `AUTHORS` file


## License

libcouchbase is licensed under the Apache 2.0 License. See `LICENSE` file for
details.
