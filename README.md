[![Pipeline](https://gitlab.com/bolderflight/software/pres/badges/main/pipeline.svg)](https://gitlab.com/bolderflight/software/pres/) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

![Bolder Flight Systems Logo](img/logo-words_75.png)

# Pres
Defines a common interface for our pressure sensors
   * [License](LICENSE.md)
   * [Changelog](CHANGELOG.md)
   * [Contributing guide](CONTRIBUTING.md)

# Description
This library defines what a *Pres* interface should look like, enabling higher level code to abstract out sensor specifics and design against this interface.

## Installation
CMake is used to build this library, which is exported as a library target called *pres*. The header is added as:

```
#include "pres/pres.h"
```

The library can be also be compiled stand-alone using the CMake idiom of creating a *build* directory and then, from within that directory issuing:

```
cmake ..
make
```

This will build the library and an example called *example*, which has source code located in *examples/example.cc*.

## Namespace
This library is within the namespace *bfs*.

## Class / Methods

**struct PresData** defines a structure of data returned from the sensor. The data fields are:

| Name | Description |
| --- | --- |
| bool new_data | Whether new data was read |
| bool healthy | Whether the pressure sensor is healthy |
| float pres_pa | The measured pressure, Pa |

Health is determined by whether the sensor fails to read 5 times in a row at the expected sampling rate.

**Pres** Concepts are used to define what a *Pres* compliant object looks like and provide a means to templating against a *Pres* interface. The required methods are:

**PresData pres_data()** This method returns the PresData from the last successful *Read*.