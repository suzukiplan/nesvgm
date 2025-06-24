# nesvgm.hpp - NES VGM Driver

このプログラムは [NSFPlay/NSFPlug](https://github.com/bbbradsmith/nsfplay) の実装から NES APU の実装部分のみを抽出して、VGM形式のパーサーを追加実装した C++ シングルヘッダ形式のファミコン標準音源エミュレータです。

VGS-Zero の実装に含まれコードの一部を利用し易い MIT ライセンスに調整したものです。

例えば、自作ゲームで [FamiStudio](https://famistudio.org/) や [Furnace](https://tildearrow.org/furnace/) などで作成したファミコン音源の BGM を再生したいケースでの利用を想定しています。

## How to Use

[nesvgm.hpp](nesvgm.hpp) をプロジェクトに追加して `#include` するだけで利用できます。

### 1. Include

```c++
#include "nesvgm.hpp"
```

### 2. Make an Instance

```c++
xgm::NesVgmDriver* nes = new xgm::NesVgmDriver();
```

### 3. Load VGM File

オンメモリにロードしたデータとデータサイズを指定して `xgm::NesVgmDriver::Load` を呼び出します。

```c++
if (nes->Load(vgmData, vgmSize)) {
    puts("success!");
    // ロード成功後にサンプリング周波数とチャネル数を設定してリセット
    nes->SetPlayFreq(44100);
    nes->SetChannels(1);
    nes->Reset();
} else {
    puts("failed!");
}
```

※VGM ファイルは __Version 1.61 以降__ でなければなりません

### 4. Render Sampling Data

`xgm::NesVgmDriver::Render` を呼び出すことで欲しいサイズのサンプリングデータを取得できます。

```c++
nes->Render(samplingBuffer, samplingNumber);
```

- 量子化単位は 16bit (2 bytes) 固定です。
- `samplingNumber` は `samplingBuffer` のサイズを 2 で割った値です。

## License

[MIT](LICENSE.txt)

> 備考: 元となる NES APU の実装は Digital Sound Antiques さんが配布しているもの（[参考](https://github.com/bbbradsmith/nsfplay/blob/master/xgm/readme_ja.txt)）で、ライセンスに関する記載がありませんが、Digital Sound Antiques さんは他のエミュレータを MIT ライセンスで配布されているため、恐らく MIT ライセンスであれば互換性があるものと想定して MIT ライセンスに設定しています。もしも、Digital Sound Antiques さんから怒られたらライセンス修正するなり公開停止するなりの対応をする可能性があります。NES APU 実装部分を含めて不明点などがある場合は[当方](https://github.com/suzukiplan/nesvgm/issues)までご報告ください。
