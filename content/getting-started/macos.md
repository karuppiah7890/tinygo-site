---
title: "macOS"
date: 2019-04-15T11:41:45+02:00
draft: false
weight: 1
---

This page has information on how to install and use TinyGo on macOS.

If you want to use TinyGo to compile your own or sample code, you can install the release version directly on your machine by following the "Quick Install" instructions below.

You can also install the full source code to the TinyGo compiler itself, generally for people who wish to contribute to the project or want to build the compiler from sources directly.

The third option is to use the Docker image. This has the benefit of making no changes to your system but has a large download and installation size. For instructions on using the Docker image, please see the page [here](../using-docker).

## Quick Install

You must have Go v1.12+ already installed on your machine in order to install TinyGo.

You can use Homebrew to install TinyGo using the following commands:

```shell
brew tap tinygo-org/tools
brew install tinygo
```

You can test that the installation is working properly by running this code which should display the version number:

```shell
$ tinygo version
tinygo version 0.6.0 darwin/amd64
```

If you are only interested in compiling TinyGo code for WebAssembly then you are done with the installation.

Otherwise, please continue with the installation of the additional requirements for your desired microcontroller.

### Additional Requirements for Microcontrollers

There are some additional requirements to compile TinyGo programs that can run on microcontrollers.

#### ARM Cortex-M

In order to develop for ARM-based microcontrollers you will need to install LLVM 8:

```shell
brew install llvm
```

#### AVR (Arduino)

If you want to compile code for AVR-based microcontrollers such as Arduino, you will need to install gcc-avr:

```shell
brew tap osx-cross/avr
brew install avr-gcc
brew install avrdude
```

## Source Install

First, obtain the TinyGo source code:

```shell
go get -d github.com/tinygo-org/tinygo
cd $GOPATH/src/github.com/tinygo-org/tinygo
```

Once you have the code, you can install the various prerequisites using [`dep`](https://golang.github.io/dep/):

```shell
dep ensure
```

You now have two options: build LLVM manually or use LLVM from Homebrew. The
advantage of a manual build is that it includes all supported targets (including
AVR) while Homebrew includes only stable targets. Unless you want to compile for
AVR-based boards, you can use Homebrew.

### With LLVM from Homebrew

The easiest way to install LLVM on macOS is through
[Homebrew](https://formulae.brew.sh/formula/llvm). Make sure you install LLVM 8:

```shell
brew install llvm@8
```

Installing TinyGo should now be as easy as:

```shell
go install
```

Note that you should not use `make` when you want to build using a
system-installed LLVM, just use the Go toolchain. `make` is used when you want
to use a self-built LLVM.

### With a self-built LLVM

You can also manually build LLVM. This is a long process which takes at least
one hour on most machines. In most cases you can build TinyGo using LLVM from
Homebrew. However, the Homebrew build does not support the experimental AVR
target so you'll have to build from source if you want to use TinyGo for the
Arduino Uno.

You will need a few extra tools that are required during the build of LLVM:

```shell
brew install cmake ninja
```

The following command takes care of downloading and building LLVM. It places the
source code in `llvm-build/` and the build output in `llvm/`. It only needs to
be done once until the next LLVM release.

```shell
make llvm-build
```

Once this is finished, you can build TinyGo against this manually built LLVM:

```shell
make
```

This results in a `tinygo` binary in the `build` directory:

```shell
$ ./build/tinygo version
tinygo version 0.6.0 darwin/amd64
```

### Additional Requirements for Microcontrollers

Before anything can be built for a bare-metal target, you need to generate some
files first:

```shell
make gen-device
```

This will generate register descriptions, interrupt vectors, and linker scripts
for various devices. Also, you may need to re-run this command after updates,
as some updates cause changes to the generated files.

The same additional requirements to compile TinyGo programs that can run on microcontrollers must be fulfilled when installing TinyGo from source. Please follow [these instructions](#additional-requirements-for-microcontrollers) above.

## Docker Install

For instructions on using the Docker image, please see the page [here](../using-docker).
