# nesvgm.hpp - NES VGM Driver

This program is a C++ single-header NES standard sound source emulator that extracts only the NES APU implementation part from the [NSFPlay/NSFPlug](https://github.com/bbbradsmith/nsfplay) implementation and implements an additional parser in [VGM format](https://vgmrips.net/wiki/VGM_Specification).

Some of the code included in the VGS-Zero implementation has been adapted to the MIT license for easy use.

For example, it is intended for use in cases where you want to play NES sound source BGM created by [FamiStudio](https://famistudio.org/) or [Furnace](https://tildearrow.org/furnace/) in your own games.

## How to Use

Just add [nesvgm.hpp](nesvgm.hpp) and [rom_tndtable.c](rom_tndtable.c) to your project and `#include "nesvgm.hpp"`.

### 1. Include

```c++
#include "nesvgm.hpp"
```

### 2. Make an Instance

```c++
xgm::NesVgmDriver* nes = new xgm::NesVgmDriver();
```

### 3. Load VGM File

Call `xgm::NesVgmDriver::Load` with the data loaded on-memory and the data size.

```c++
if (nes->Load(vgmData, vgmSize)) {
    puts("success!");
    // Reset with sampling frequency and number of channels after successful load
    nes->SetPlayFreq(44100);
    nes->SetChannels(1);
    nes->Reset();
} else {
    puts("failed!");
}
```

VGM files must be __Version 1.61 or later__.

### 4. Render Sampling Data

You can call `xgm::NesVgmDriver::Render` to get sampled data of the size you want.

```c++
nes->Render(samplingBuffer, samplingNumber);
```

- The quantization unit is fixed at 16 bits (2 bytes).
- The `samplingNumber` is the size of `samplingBuffer` divided by 2.

## License

[MIT](LICENSE.txt)

> Note: The original NES APU implementation is distributed by Digital Sound Antiques ([reference](https://github.com/bbbradsmith/nsfplay/blob/master/xgm/)), There is no license information, but since Digital Sound Antiques distributes other emulators under MIT license, I assume that the MIT license is compatible with this implementation. If there are any unclear points including the NES APU implementation, please [contact us](https://github.com/suzukiplan/nesvgm/issues).
