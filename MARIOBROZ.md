# Lichii (mariobroz's version)

A custom build of [Lichii](https://github.com/zortos293/Lichii) by **mariobroz**,
fine-tuned for *The Legend of Zelda: Ocarina of Time* (Switch 2) on AMD Radeon GPUs (tested on Ryzen 7 5700X3D + RX 7700 XT).
All credit for the emulator itself goes to the Lichii / Eglantine / Eden / yuzu authors. This is an unofficial custom build.

## Improvements & Fixes
- **Ocarina of Time Kokiri Forest Sky & Mist Fix**: Folded integer and floating-point multiplications by 0 and 1 in `constant_propagation_pass.cpp` (`FoldIMul32` and `FoldFPMul32`). This allows the sky ambient compute pass (`b76d7b3e9e93093d`) to properly identify its storage buffer base address in `global_memory_to_storage_buffer_pass.cpp`, eliminating the blown-out HDR background and resolving the white mist screen washout completely at root cause without needing `EDEN_NX2_REJECT_HASH`.
- **Faster tensor-op emulation on AMD Radeon**: title screen ~15 -> 50-60 fps.
- **Fewer waits between GPU channels**: semaphore acquires resume as soon as a matching release is queued.
- **Cheaper texture-cache garbage collection**: `EDEN_NX2_TC_MIN_AGE` keeps recently used images (avoids flashes).

## Configurações NX2 recomendadas para Ocarina of Time
O arquivo `nx2_switches.txt` já vem configurado na raiz da build e em `%APPDATA%\eden\nx2_switches.txt` com:
- `EDEN_NX2_TC_MIN_AGE=600` (evita thrashing de texturas na Floresta Kokiri)
- `EDEN_NX2_RESUME_ADD=0` (evita oscilação de sincronização GPU compute)
- `EDEN_NX2_SPLIT=2`
- `EDEN_NX2_FENCE_BATCH=1`
- `EDEN_NX2_REJECT_HASH=c72b6ea746c00433,3ddfb1d7a854c849` (rejeita a névoa densa de chão e os feixes de luz que cobriam o cenário)

## License
GPL-3.0-or-later, like Lichii and Eglantine. Full source code included.
