# Building Dolphin-ScoobyAudio (2609a + patch)

Tested on Windows with **Visual Studio 2026 Build Tools** (MSVC) and CMake/Ninja from that install.

1. Clone Dolphin at tag **2609a** and submodules:

   ```powershell
   git clone --depth 1 --branch 2609a https://github.com/dolphin-emu/dolphin.git
   cd dolphin
   git submodule update --init --recursive
   ```

2. Apply the patch from this repo:

   ```powershell
   git apply C:\path\to\dolphin-scooby-hle-audio\patches\dolphin-2609a-ax-adpcm-stream-loop.patch
   ```

3. Configure and build (example — use your VS `vcvars64.bat`):

   ```powershell
   cmake -S . -B build -G Ninja -DCMAKE_BUILD_TYPE=Release
   cmake --build build --parallel
   ```

4. Copy `build/Binary/x64/Dolphin.exe` (or the `Dolphin.exe` your tree produces) beside an existing **Dolphin 2609a** portable kit as `Dolphin-ScoobyAudio.exe`. Do not mix DLLs from a different Dolphin version.

Upstream build docs: [Building for Windows](https://github.com/dolphin-emu/dolphin/wiki/Building-for-Windows).
