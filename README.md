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
cmake .. -DMCU=MK66FX1M0
make
```

This will build the library and an example called *example*, which has source code located in *examples/example.cc*. Notice that the *cmake* command includes a define specifying the microcontroller the code is being compiled for. This is required to correctly configure the code, CPU frequency, and compile/linker options. The available MCUs are:
   * MK20DX128
   * MK20DX256
   * MK64FX512
   * MK66FX1M0
   * MKL26Z64
   * IMXRT1062_T40
   * IMXRT1062_T41

These are known to work with the same packages used in Teensy products. Also switching packages is known to work well, as long as it's only a package change.

## Namespace
This library is within the namespace *bfs*.

## Class / Methods

**struct PresConfig** defines a structure used to configure the sensor. The data fields are:

| Name | Description |
| --- | --- |
| std::variant<TwoWire &ast;, SPIClass &ast;> bus | A pointer to the interface used to the communicate with the sensor |
| int8_t dev | The I2C address or SPI pin |
| int16_t sampling_period_ms | The sampling period for the sensor, used to determine sensor health |

**struct PresData** defines a structure of data returned from the sensor. The data fields are:

| Name | Description |
| --- | --- |
| bool new_data | Whether new data was read |
| bool healthy | Whether the pressure sensor is healthy |
| float pres_pa | The measured pressure, Pa |

Health is determined by whether the sensor fails to read 5 times in a row at the expected sampling rate.

**Pres** Concepts are used to define what a *Pres* compliant object looks like and provide a means to templating against a *Pres* interface. The two required methods are:

**bool Init(const PresConfig &ref)** This method should receive an *PresConfig* struct and should establish communication with the pressure sensor, configure the sensor, and zero biases. True is returned on successfully initializing the sensor.

**bool Read(PresData &ast; const ptr)** This method should get new data from the sensor and return it using a pointer to the *PresData* struct. True is returned if new data is received.