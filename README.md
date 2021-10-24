# pres
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

**struct PresConfig** defines a structure used to configure the sensor. The data fields are:

| Name | Description |
| --- | --- |
| std::optional<int8_t> transducer | (optional) The pressure transducer |

Some sensor drivers, such as the AMS5915 or AMS5812, need the pressure transducer specified to set the appropriate pressure ranges. The optional transducer parameter is available for that purpose.

**struct PresData** defines a structure of data returned from the sensor. The data fields are:

| Name | Description |
| --- | --- |
| bool new_data | Whether new data was read |
| bool healthy | Whether the pressure sensor is healthy |
| float pres_pa | The measured pressure, Pa |

Health is determined by whether the sensor fails to read 5 times in a row at the expected sampling rate.

**Pres** Concepts are used to define what a *Pres* compliant object looks like and provide a means to templating against a *Pres* interface. The required methods are:

**bool Config(const PresConfig &ref)** This method should receive a PresConfig struct and setup the sensor driver configuration. Note that the configuration should be applied in the *Init* method, this simply checks the configuration for validity and sets up the sensor driver object. True is returned if the config is valid, otherwise false if returned.

**PresData pres_data()** This method returns the PresData from the last successful *Read*.