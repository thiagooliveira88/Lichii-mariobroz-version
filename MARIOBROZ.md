# Lichii (mariobroz's version)

A custom build of [Lichii](https://github.com/zortos293/Lichii) by **mariobroz**,
fine-tuned for *The Legend of Zelda: Ocarina of Time* (Switch 2) on AMD Radeon GPUs (tested on Ryzen 7 5700X3D + RX 7700 XT).
All credit for the emulator itself goes to the Lichii / Eglantine / Eden / yuzu authors. This is an unofficial custom build.

## Improvements & Fixes (Build 2)
- **Eliminação do Véu Branco e Névoa Cega**:
  Configuração padrão com rejeição dos dois shaders corrompidos de névoa (`c72b6ea746c00433` - ground mist, e `3ddfb1d7a854c849` - light shafts/fog veil).
- **Correção da Superexposição / Brilho Estourado**:
  - Implementado o recombined folding em `constant_propagation_pass.cpp` (`FoldHalvesRecombine` e `FoldIMul32`), permitindo que o shader de compute de iluminação ambiente do céu (`b76d7b3e9e93093d`) vincule corretamente seu buffer de armazenamento e forneça os harmônicos esféricos reais em vez de zeros.
  - Correção na decodificação do opcode HMMA de matrizes (`0x23c`) em `lower_to_maxwell.h`, viabilizando a execução compacta da rede neural de pós-processamento de 11 passos do jogo, reduzindo a luma excessiva e restaurando o contraste nativo do ambiente.
- **Correção da Cintilação / Piscadas de Materiais (Sentilação)**:
  - Re-upload de bancos uniformes nativos a cada bind em `buffer_cache.h` (`EDEN_NX2_BANK_REUPLOAD`), eliminando dados obsoletos entre draw calls que causavam flashes brancos nas roupas do Link, folhas e reflexos de água.
  - Relaxamento de testes de profundidade bit-exact (`EQUAL` para `LESS_OR_EQUAL` via `EDEN_NX2_DEPTH_EQUAL_RELAX=1`) para estabilizar a renderização e eliminar z-fighting em placas AMD Radeon.
  - Preservação de texturas ASTC não compactadas (`EDEN_NX2_KEEP_ASTC=1`) para placas com 12 GB+ de VRAM, eliminando ruído e faíscas de compressão BC3.
- **Otimizações para AMD Radeon**:
  - Offload de macros GPU (`EDEN_NX2_SPLIT=2`) e loteamento de cercas de sincronização (`EDEN_NX2_FENCE_BATCH=1`).
  - Prevenção de thrashing no cache de texturas (`EDEN_NX2_TC_MIN_AGE=600`).

## Configurações NX2 incluídas por padrão (`nx2_switches.txt`)
O arquivo `nx2_switches.txt` já vem configurado na raiz da build com:
- `EDEN_NX2_SPLIT=2`
- `EDEN_NX2_FENCE_BATCH=1`
- `EDEN_NX2_TC_MIN_AGE=600`
- `EDEN_NX2_RESUME_ADD=0`
- `EDEN_NX2_KEEP_ASTC=1`
- `EDEN_NX2_DEPTH_EQUAL_RELAX=1`
- `EDEN_NX2_REJECT_HASH=c72b6ea746c00433,3ddfb1d7a854c849`

## License
GPL-3.0-or-later, like Lichii and Eglantine. Full source code included.
