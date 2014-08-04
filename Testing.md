### Testing the C++ Driver

If you contribute to the C++ driver, you'll need to test your changes.  The driver comes with a number of tests to ensure its functionality and performance.  There are a few different kinds of tests within the driver's codebase.

Note: if you are running Mavericks, you may need to include the ```--osx-version-min=10.9``` flag to the commands below.

### Unit tests

Unit tests do not require a running mongod, and in fact will not run if you have a mongod running locally on port 27000.  These tests are designed to test individual components of the driver in isolation, and the test files are found in the same directory as the things they test (so, ```bson_validate.cpp``` and ```bson_validate_test.cpp``` are both found in ```src/mongo/bson```).  The different unit tests are listed [here](https://github.com/mongodb/mongo-cxx-driver/blob/legacy/src/mongo/SConscript#L36-L62).

Build all the unit tests with scons:

```
> scons unittests
```

Build and run all the unit tests:

```
> scons test
```

Build an individual unit test:

```
> scons build-full/test/name
```

Build and run an individual unit test:

```
> scons run-full/test/name
```

### Integration Tests

Integration tests require a local mongod to be running on port 27999.  These tests are located in ```src/mongo/unittest```.  There is a list of the different integration tests [here](https://github.com/mongodb/mongo-cxx-driver/blob/legacy/src/mongo/SConscript#L87-L93).

Build all the integration tests:

```
> scons integration_tests
```

Build and run all the integration tests:

```
> scons integration
```

Individual integration tests can be run in the same way as individual unit tests, shown above.

### Helpful Scons Flags

```--osx-version-min=10.9``` - needed for OSX Mavericks.

```--cache``` - use an object cache for builds.
