---
layout: default
title: cpr - C++ Requests
---

# Curl for People

C++ Requests is a simple wrapper around [libcurl](http://curl.haxx.se/libcurl) inspired by the excellent [Python Requests](https://github.com/kennethreitz/requests) project.

Despite its name, libcurl's easy interface is anything but, and making mistakes misusing it is a common source of error and frustration. Using the more expressive language facilities of C++11, this library captures the essence of making network calls into a few concise idioms.

Here's a quick GET request:

{% raw %}
```c++
#include <cpr/cpr.h>

int main(int argc, char** argv) {
    cpr::Response r = cpr::Get(cpr::Url{"https://api.github.com/repos/libcpr/cpr/contributors"},
                      cpr::Authentication{"user", "pass"},
                      cpr::Parameters{{"anon", "true"}, {"key", "value"}});
    r.status_code;                  // 200
    r.header["content-type"];       // application/json; charset=utf-8
    r.text;                         // JSON text string
}
```
{% endraw %}

And here's [less functional, more complicated code, without cpr](https://gist.github.com/whoshuu/2dc858b8730079602044).

## Features

C++ Requests currently supports:

* Custom headers
* Url encoded parameters
* Url encoded POST values
* Multipart form POST upload
* File POST upload
* Basic authentication
* Digest authentication
* NTLM authentication
* Connection and request timeout specification
* Timeout for low speed connection
* Asynchronous requests
* :cookie: support!
* Proxy support
* Callback interfaces
* PUT methods
* DELETE methods
* HEAD methods
* OPTIONS methods
* PATCH methods
* Thread Safe access to [libCurl](https://curl.haxx.se/libcurl/c/threadsafe.html)
* OpenSSL and WinSSL support for HTTPS requests

## Planned

Support for the following will be forthcoming (in rough order of implementation priority):

* [Streamed requests](https://github.com/libcpr/cpr/issues/25)

and much more!

## Usage

### CMake

If you already have a CMake project you need to integrate C++ Requests with, the primary way is to use `fetch_content`.
Add the following to your `CMakeLists.txt`.


```cmake
include(FetchContent)
FetchContent_Declare(cpr GIT_REPOSITORY https://github.com/libcpr/cpr.git
                         GIT_TAG beb9e98806bb84bcc130a2cebfbcbbc6ce62b335) # The commit hash for 1.7.2. Replace with the latest from: https://github.com/libcpr/cpr/releases
FetchContent_MakeAvailable(cpr)
```

This will produce the target `cpr::cpr` which you can link against the typical way:

```cmake
target_link_libraries(your_target_name PRIVATE cpr::cpr)
```

That should do it!
There's no need to handle `libcurl` yourself. All dependencies are taken care of for you.  
All of this can be found in an example [**here**](https://github.com/libcpr/example-cmake-fetch-content).

### Packages for Linux Distributions

Alternatively, you may install a package specific to your Linux distribution. Since so few distributions currently have a package for cpr, most users will not be able to run your program with this approach.

Currently, we are aware of packages for the following distributions:

* [Arch Linux (AUR)](https://aur.archlinux.org/packages/cpr)

If there's no package for your distribution, try making one! If you do, and it is added to your distribution's repositories, please submit a pull request to add it to the list above. However, please only do this if you plan to actively maintain the package.

## Requirements

The only explicit requirements are:

* a `C++11` compatible compiler such as Clang or GCC. The minimum required version of GCC is unknown, so if anyone has trouble building this library with a specific version of GCC, do let me know
* If you would like to perform https requests `OpenSSL` and its development libraries are required.

## Building cpr - Using vcpkg

You can download and install cpr using the [vcpkg](https://github.com/Microsoft/vcpkg) dependency manager:
```Bash
git clone https://github.com/Microsoft/vcpkg.git
cd vcpkg
./bootstrap-vcpkg.sh
./vcpkg integrate install
./vcpkg install cpr
```
The `cpr` port in vcpkg is kept up to date by Microsoft team members and community contributors. If the version is out of date, please [create an issue or pull request](https://github.com/Microsoft/vcpkg) on the vcpkg repository.

## Building cpr - Using Conan

You can download and install `cpr` using the [Conan](https://conan.io/) package manager. Setup your CMakeLists.txt (see [Conan documentation](https://docs.conan.io/en/latest/integrations/build_system.html) on how to use MSBuild, Meson and others).
An example can be found [**here**](https://github.com/libcpr/example-cmake-conan).

The `cpr` package in Conan is kept up to date by Conan contributors. If the version is out of date, please [create an issue or pull request](https://github.com/conan-io/conan-center-index) on the `conan-center-index` repository.

## Contributing

Please fork this repository and contribute back using [pull requests](https://github.com/libcpr/cpr/pulls). Features can be requested using [issues](https://github.com/libcpr/cpr/issues). All code, comments, and critiques are greatly appreciated.
