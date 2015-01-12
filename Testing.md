### Testing the Legacy C++ Driver

If you contribute to the C++ driver, you'll need to test your changes.  The driver comes with a number of tests to ensure its functionality and performance.  There are a few different kinds of tests within the driver's codebase.

Note: if you are running OS X Mavericks or above, you may need to include the ```--osx-version-min=10.9``` flag to the commands below.

### Unit tests

Unit tests do not require a running mongod, and in fact will not run if you have a mongod running locally on port 27000.  These tests are designed to test individual components of the driver in isolation, and the test files are found in the same directory as the things they test (so, ```bson_validate.cpp``` and ```bson_validate_test.cpp``` are both found in ```src/mongo/bson```).  The different unit tests are listed [here](https://github.com/mongodb/mongo-cxx-driver/blob/legacy/src/mongo/SConscript#L36-L62).

Build all the unit tests with scons:

```
> scons build-unit
```

Build and run all the unit tests:

```
> scons unit
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

Integration tests must run against [Mongo Orchestration](https://github.com/10gen/mongo-orchestration).  Install and setup Mongo Orchestration as follows:
```
> git clone https://github.com/10gen/mongo-orchestration.git
> cd mongo-orchestration
> python setup.py install
```
To run Mongo Orchestration, you'll need to alter the provided config file to fit your system.  Open ```mongo-orchestration.config``` and replace the paths there with paths to your MongoDB binaries.  It is only required to have one MongoDB version defined, so feel free to keep one of the ```"releases"``` and delete the other entries.

Start up Mongo Orchestration and leave it running in the background while you run the integration test suite:
```
> mongo-orchestration -f mongo-orchestration.config -e <yourMongoVersion> start
```

The integration tests are located in ```src/mongo/unittest```.  Additionally, some tests require the parameter ```enableTestCommands``` to be set. There is a list of the different integration tests [here](https://github.com/mongodb/mongo-cxx-driver/blob/legacy/src/mongo/SConscript#L34-L47).

To build all the integration tests:

```
> scons build-integration
```

Build and run all the integration tests:

```
> scons integration
```

Individual integration tests can be run in the same way as individual unit tests, shown above.

Note: to run the SASL integration tests, you should build with the ```--use-sasl-client``` flag.

### Client Example Programs

The driver includes a number of example programs of its use.  The examples are listed [here](https://github.com/mongodb/mongo-cxx-driver/blob/legacy/src/SConscript.client#L158-L171), and the source files are found in ```src/mongo/client/examples```.  The examples expect a mongod to be running locally on port 27999.

Build the examples with scons:

```
> scons build-examples
```

Build and run the examples:

```
> scons examples
```
### Run all tests
Run the unit tests, integration tests, and examples with scons:
```
> scons test
```
### Helpful Scons Flags

```--cache``` - use an object cache for builds.
