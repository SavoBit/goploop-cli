# goploop-cli [![GoDoc](https://godoc.org/github.com/kolyshkin/goploop-cli?status.png)](https://godoc.org/github.com/kolyshkin/goploop-cli)

This is a Go wrapper for [ploop](https://github.com/kolyshkin/ploop),
a command line tool to manage ploop. It is designed to be a drop-in
replacement for [goploop](https://github.com/kolyshkin/goploop) for
those who don't like goploop build/link dependencies on a few C libraries.

## Changes from goploop

The differences between this package and [goploop](https://github.com/kolyshkin/goploop)
are:

* goploop calls ploop C library, while goploop-cli calls ploop command line tool
* goploop has a few build time dependencies, goploop-cli has no such requirements
* goploop has no runtime dependencies if built statically; goploop-cli requires ploop tool during runtime
* goploop is probably more efficient than goploop-cli
* goploop-cli currently have some limitations (see below)

The following methods are currently not implemented, compared to goploop:
* ``SetVerboseLevel()`` don't do anything
* ``SetLogFile()`` and ``SetLogLevel()`` are missing
* ``GetTopDelta()`` is missing

The following methods differ:
* ``GetFSInfo()`` returns BlockSize=1K instead of actual filesystem block size
* ``Resize()`` ignores ``offline`` argument (ploop tool automatically chooses whether to do online or offline resize)

## What is ploop?

Ploop is a loopback block device (a.k.a. "filesystem in a file"),
not unlike [loop](https://en.wikipedia.org/wiki/Loop_device)
but with better performance and more features, including:

* thin provisioning (image grows on demand)
* dynamic resize (both grow and shrink)
* instant online snapshots
* online snapshot merge
* optimized image migration with write tracker (ploop copy)

Ploop is implemented in the kernel and is currently available
in OpenVZ RHEL6 and RHEL7 based kernels. For more information
about ploop, see [openvz.org/Ploop](https://openvz.org/Ploop).

## Prerequisites

You need to have
* ext4 formatted partition (note RHEL/CentOS 7 installer uses xfs by default, that won't work!)
* ploop-enabled kernel installed
* ploop package installed

Currently, all the above comes with OpenVZ, please see [openvz.org/Quick_installation](https://openvz.org/Quick_installation).

## Usage

This package is used by Docker ploop graphdriver, see https://github.com/kolyshkin/docker/tree/ploop/daemon/graphdriver/ploop

For primitive examples of how to use the package, see [ploop_test.go](ploop_test.go).

## See also

* [goploop](https://github.com/kolyshkin/goploop) -- an alternative implementation using libploop
* [openvz.org/Ploop](https://openvz.org/Ploop) -- more information about ploop
* [ploop(8)](https://openvz.org/Man/ploop.8) man page
* [OpenVZ](https://openvz.org)
