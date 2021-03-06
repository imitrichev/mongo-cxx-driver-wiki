## (New) Driver Documentation

*Note: the new driver, which is still in development, can be found in the [master branch](https://github.com/mongodb/mongo-cxx-driver/tree/master).*

- [Quickstart Guide](Quickstart-Guide-(New-Driver))
- [Library Thread Safety](Library-Thread-Safety) - A must read for anyone writing a multithreaded program with the library.

### Maintainer Notes

- [C++ Driver Versioning Scheme](C---Driver-Versioning-Scheme)

## Legacy Driver Documentation

The legacy C++ driver builds on x86 and x86-64 architectures for Linux, Mac OS X, Windows, FreeBSD and Solaris.

The Legacy MongoDB C++ driver library includes a bson package that implements the BSON specification (see http://www.bsonspec.org). This library can be used standalone for object serialization and deserialization even when one is not using MongoDB at all.

### Getting Started
 - [Download and Compile the Legacy Driver](Download-and-Compile-the-Legacy-Driver)
 - Configuring the driver:
       - [Configuring the legacy driver](Configuring-the-Legacy-Driver) (0.9+)
       - [Configuring the legacy-26compat driver](Configuring the 26Compat Driver)
 - [Getting Started with the legacy C++ Driver](Tutorial)
 - [Legacy BSON Helper Functions](BSON Helper Functions)
 - [Breaking changes between 26compat and legacy](Breaking changes between 26compat and legacy)

### Testing
 - [Testing the legacy driver](Testing)

### Documentation
 - [Legacy API Documentation](http://api.mongodb.org/cxx/)
