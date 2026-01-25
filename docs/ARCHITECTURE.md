## Purpose
High-level overview of the Dataset Maker flow and how the main modules connect.

## Main entrypoint
- `gradio_interface.py` is the primary UI and orchestration layer.

## Data flow (UI)
1. Create project -> `datasets_folder/<project>/`.
2. Upload audio -> `wavs/`.
3. Optional combine -> `combined.wav` batches (and move originals to `uncombined_wavs/`).
4. Transcribe:
   - Silence/WhisperX (Silero VAD) -> `transcribe/` audio segments + `train_text_files/train.txt`.
   - Emilia pipe -> `<project>_emilia_dataset/` with `audio/` clips and `<project>_transcribed.jsonl`.
5. Optional cleanup/correction -> revised `train.txt` (non-Emilia flow).
6. Export -> dataset folders for Base, Higgs, VibeVoice, or Qwen 3 TTS formats (uses `transcribe/`).
   - Qwen 3 TTS reference audio selection is sourced from `transcribe/` files in the UI.

## Key modules used by the UI
This list is intentionally kept here (not in README) since it is implementation-facing.
- `transcriber.py`: Slicing and transcription logic (silence slicer, WhisperX, or Emilia).
- `slicer2.py`: Silence-based audio segmentation utility.
- `emilia_pipeline.py`: Multi-stage pipeline (UVR -> diarization -> VAD -> WhisperX -> DNSMOS).
- `infer_uvr.py`: UVR/MDX model wrapper used for vocal separation.
- `llm_reformatter_script.py`: Optional transcript cleanup using a local Llama model.
- `safe_globals.py`: Torch safe deserialization registry for model loading.

## Export formats
- Base: `train.txt` + wavs.
- Higgs: `metadata.json` + per-sample `.wav/.txt`.
- VibeVoice: `.jsonl` manifest + wavs.
- Qwen 3 TTS: `.jsonl` manifest with `audio`, `text`, and `ref_audio` pointing into `data/`.
- Emilia pipe output: `<project>_emilia_dataset/` JSONL + audio clips (intended for IndexTTS 2, not exclusive).
