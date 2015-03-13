The goal of this guide is to get you to the point where you can build a simple application with the driver
in a few short minutes.

## Prerequisites

1. *nix platform. We have only tested the driver on Linux and OSX.
2. A modern compiler. We recommend `clang++` 3.4+ or `g++` 4.8+.
3. CMake 3.1+. 

## Build and install the C driver

The C++ driver uses [libbson](https://github.com/mongodb/libbson) and the [MongoDB C driver](https://github.com/mongodb/mongo-c-driver) internally. The C driver will install libbson if it isn't already present.

1. Install the C driver prerequisites `automake`, `autoconf` and `libtool`. See the C-driver [README](https://github.com/mongodb/mongo-c-driver/blob/master/README.rst) for further instructions if you run into trouble.

2. Clone the C driver. Currently we depend on APIs in the `1.2.0-dev` branch.

`git clone -b 1.2.0-dev https://github.com/mongodb/mongo-c-driver`

3. Build and install the C driver.

`cd mongo-c-driver`

`./autogen.sh`

`make && sudo make install`

## Build the C++ driver

1. Clone the repository.

`git clone -b master https://github.com/mongodb/mongo-cxx-driver`

2. Build the driver.

`cd mongo-cxx-driver/build`

`cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr/local ..`

`sudo make && sudo make install`

3. Fire up your editor and copy in this code to a file called `hellomongo.cpp`:
```c++
#include <iostream>

#include <bsoncxx/builder/stream/document.hpp>
#include <bsoncxx/json.hpp>

#include <mongocxx/client.hpp>
#include <mongocxx/instance.hpp>

int main(int, char**) {
    mongocxx::instance inst{};
    mongocxx::client conn{};

    auto collection = conn["testdb"]["testcollection"];
    auto document = document{} << "hello" << "world";

    collection.insert_one(document.view());
    auto cursor = collection.find({});
    
    for (auto&& doc : cursor) {
        std::cout << bsoncxx::to_json(doc) << std::endl;
    }
}
```

You can compile with:

`c++ --std=c++11 hellomongo.cpp -o hellomongo $(pkg-config --cflags --libs mongocxx)`
 

