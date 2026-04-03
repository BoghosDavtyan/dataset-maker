# Dataset Maker
Multi-purpose dataset maker for various TTS models:
- Tortoise TTS/XTTS
- StyleTTS 2 ~ [Webui](https://github.com/JarodMica/StyleTTS-WebUI)
- Higgs Audio ~ [Base](https://github.com/JimmyMa99/train-higgs-audio) - [My fork](https://github.com/JarodMica/higgs-audio/tree/training)
- VibeVoice ~ [Base](https://github.com/voicepowered-ai/VibeVoice-finetuning) - [My fork](https://github.com/JarodMica/VibeVoice-finetuning)
- IndexTTS 2 ~ [My Trainer](https://github.com/JarodMica/index-tts/tree/training_v2)

## Main entrypoint
The primary script is `gradio_interface.py`. It starts the Gradio UI that orchestrates
project creation, audio uploads, transcription, and dataset export.

## Quick start (Windows)
1. Make sure you have Astral `uv` installed.
2. Run:
   ```bash
   git clone https://github.com/JarodMica/dataset-maker.git
   cd dataset-maker
   uv sync
   ```
3. Because `flash-attn` requires specific build isolation flags to compile properly, run this manually:
   ```bash
   uv pip install flash-attn --no-build-isolation
   ```
4. Launch the UI:
   ```bash
   uv run .\gradio_interface.py
   ```

## Core workflow
1. Create a project in the UI (creates `datasets_folder/<project>`).
2. Upload audio into `wavs/`.
3. Optionally combine short clips into longer files for faster transcription.
4. Transcribe using:
   - **Qwen3-ASR (Silence Slicer)**: The new default. Uses `OzLabs/Caspi-1.7B` with GPU batching for lightning-fast transcription and automatic number spelling.
   - WhisperX timestamps (Silero VAD hardcoded for pyannote 4.0 compatibility)
   - Silence slicer (local slicer + WhisperX)
   - Emilia pipe (full multi-stage pipeline)
5. Review/correct transcripts and export to the target dataset format.

## Outputs
Below are examples based on the export code in `gradio_interface.py`.

**Base (Tortoise/XTTS)** exports a `train.txt` with `file_id | transcript` and a `wavs/` folder:
```
train.txt
seg000001.wav|Hello world.
seg000002.wav|Another line.
```

**StyleTTS** format (from the Adjust train.txt tab) adds a speaker tag:
```
train.txt
seg000001.wav|Hello world.|speaker_01
```

**GPTSoVITS** format (from the Adjust train.txt tab) adds slicer_opt + language:
```
train.txt
seg000001.wav|slicer_opt|en|Hello world.
```

**Higgs Audio** export writes per-sample files and `metadata.json`:
```
speaker_01_000000.wav
speaker_01_000000.txt
metadata.json
```
`metadata.json` entries include speaker and quality fields (e.g. `speaker_id`, `gender`, `duration`, `quality_score`).

**VibeVoice** export writes a JSONL manifest with a "Speaker 0: " prefix:
```
{"text":"Speaker 0: Hello world.","audio":"<dataset_folder>/vibevoice_000000.wav"}
```

**Qwen 3 TTS** export writes a JSONL manifest with audio + ref_audio paths (relative to the export folder). Audio files are copied from the transcribe outputs into `data/` (prefixed if collisions), and the reference clip is copied as `data/ref_audio.<ext>`:
```
{"audio":"./data/seg_00000001.wav","text":"Hello world.","ref_audio":"./data/ref_audio.wav"}
```
In the UI, the Qwen 3 TTS reference audio picker lists files from the project's `transcribe/` folder.
Selecting the Qwen 3 TTS export format refreshes the reference list and resets to the first available item.

**Emilia pipe (Originally intended for IndexTTS 2, not exclusive)** writes a project-scoped JSONL and audio folder:
```
{"id":"<clip_id>","text":"Hello world.","audio":"<project>_emilia_dataset/audio/<clip_id>.mp3","speaker":"<base>_SPEAKER_X","language":"en","duration":1.23,"source":"original.wav"}
```

## Emilia configuration
The Emilia path in the UI expects `Emilia/config.json`. Copy
`Emilia/config_example.json` to `Emilia/config.json` and update:
- `huggingface_token`
- `entrypoint.input_folder_path` (optional override from UI)
- model paths in `separate.step1.model_path` and `mos_model.primary_model_path`

## Onnx Runtime Issue CUDA
CUDAExecution provider may not be found even when using uv. The fix is to remove
and then add `optimum[onnxruntime-gpu]` in the terminal.

### Problematic
```
uv run python
>>> import onnxruntime as ort
>>> print("Available providers:", ort.get_available_providers())
Available providers: ['AzureExecutionProvider', 'CPUExecutionProvider']
```

### Fixed
```
uv run python
>>> import onnxruntime as ort
>>> print("Available providers:", ort.get_available_providers())
Available providers: ['TensorrtExecutionProvider', 'CUDAExecutionProvider', 'CPUExecutionProvider']
```

## More documentation
See `docs/CONFIG.md`, `docs/ARCHITECTURE.md`, `docs/OPERATIONS.md`, and
`docs/TROUBLESHOOTING.md` for details.
