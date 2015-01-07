# Overview

The 26compat release series tracks the server 2.6 releases one-to-one. As a result, it receives only bugfixes and small updates necessary to keep it building in isolation.

The legacy release series, on the other hand, is under active development. Our philosophy is to keep the legacy branch as close to the 26compat branch as is reasonable, but that when weighing new features against compatibility, we will choose new features. As a result the legacy branch is not 100% source compatible with the 26compat branch.

This page attempts to serve as a transition guide for those users looking to migrate from the 26compat branch to the legacy branch. Note that it does *not* discuss new features in detail and simply points to the per-release notes.

> **NOTE**: This is a living document, and tracks the current state of the legacy branch. While we have attempted to capture the big ticket breakages here, there are likely to be small ones that we have missed. The C++ driver developers would appreciate it greatly if, when you are moving from 26compat to legacy, you find issues not tracked here: please send us a pull request so we can keep this page up to date and useful.

**Warning: The legacy branch is currently unstable and under active development.**

# Breaking Changes

## Changes to the build system

Scons targets have been renamed to more 'obvious' names, and some unused or unneeded targets have been removed.

### Summary

Task                                  | Scons Target
--------------------------------------|-----------------
compile driver                        |`driver`
install driver                        |`install`
check driver install (used internally)|`check-install`
build unit tests                      |`build-unit`
run unit tests                        |`unit`
build integration tests               |`build-integration`
run integration tests                 |`integration`
build client examples                 |`build-examples`
run client examples                   |`examples`
build everything (driver, unit tests, integration tests, examples|`all`
run all tests and client examples     |`test`

### Details

* The `driver` target has been created to built the client library without installing it
* The `install-mongoclient` target has been renamed to `install`
* Unit tests are now built with `build-unit`, and run with `unit`
* Integration tests are now built with `build-integration`, and run with `integration`
* Examples are now built with `build-examples`, and run with `examples`
* On OSX the `--osx-version-min` flag will now default to the current OSX version
* The `--full` flag is no longer required, and it is an error to specify it.
* The `--d` and `--dd` flags have been removed. Use the `--opt` and `--dbg` flags instead.
* The `--use-system-boost` flag is no longer required, and it is an error to specify it.
* All ABI affecting macros are now defined in a generated `config.h` header that is automatically included from `dbclient.h` and `bson.h`.
* Many server specific build options (that were unlikely to have been used when building the driver) have been removed.
* The default installation prefix is now `build/install`, rather than `/usr/local`.
* All build artifacts are now captured under the `build` directory.

## Changes to APIs
* The `mongo::BSONBuilderBase` class has been removed and is no longer a base class of `mongo::BSONObjBuilder` or `mongo::BSONArrayBuilder`
* The `OpTime` class has been completely removed. It has been replaced by the simplified `Timestamp_t` class.
* The `globalServerOptions` and `globalSSLOptions` objects and their classes have been removed. All driver configuration should be done through the new `mongo::client::Options` object.
* The `RamLog`, `RotatableFileAppender`, and `Console` classes have been removed from the logging subsystem.
* In addition, many auxiliary types, functions, and headers that were either unused, or minimally used, have been removed from the distribution.
* The `ensureIndex` and related methods have been removed. The replacement is the new `createIndex` method.
* The `QUERY` macro has been replaced by `MONGO_QUERY`.
* The `ConnectionString::parse` method now requires it's argument to be in the MongoDB URL ("mongodb://...") format. To use the old format, use the new `ConnectionString::parseDeprecated` method.
* The `ConnectionPool` and `ScopedDbConnection` classes have been removed.

## Behavior Changes
* The driver is now unlikely to function correctly unless `mongo::client::initialize` is invoked before using the driver APIs.
* The driver no longer logs any output by default. You may configure and inject a logger to re-enable logging. See `src/mongo/client/examples/clientTest.cpp` for an example of how to enable logging.
* Writes are now "[acknowledged](http://docs.mongodb.org/manual/core/write-concern/#write-concern-acknowledged)" by default. In all previous releases the default write concern was “[unacknowledged](http://docs.mongodb.org/manual/core/write-concern/#unacknowledged)”. This change may have performance and behavior implications for existing applications that did not confirm writes. You can read more about the change [here](http://docs.mongodb.org/manual/release-notes/drivers-write-concern/#driver-write-concern-change).
* The default shutdown grace period is now zero which means the driver may block forever until a successful shutdown occurs.

# Improvements

Please see the release notes for the individual legacy branch releases for details on improvements in each release:

* [legacy-1.0.0-rc3](https://github.com/mongodb/mongo-cxx-driver/releases/tag/legacy-1.0.0-rc3)
* [legacy-1.0.0-rc2](https://github.com/mongodb/mongo-cxx-driver/releases/tag/legacy-1.0.0-rc2)
* [legacy-1.0.0-rc1](https://github.com/mongodb/mongo-cxx-driver/releases/tag/legacy-1.0.0-rc1)
* [legacy-1.0.0-rc0](https://github.com/mongodb/mongo-cxx-driver/releases/tag/legacy-1.0.0-rc0)
* [legacy-0.11.0](https://github.com/mongodb/mongo-cxx-driver/releases/tag/legacy-0.11.0)
* [legacy-0.10.0](https://github.com/mongodb/mongo-cxx-driver/releases/tag/legacy-0.10.0)
* [legacy-0.9.0](https://github.com/mongodb/mongo-cxx-driver/releases/tag/legacy-0.9.0)
* [legacy-0.8.0](https://github.com/mongodb/mongo-cxx-driver/releases/tag/legacy-0.8.0)