## Purpose
How to run the project and operate its main workflows.

## Run the UI
```bash
uv sync
uv run .\gradio_interface.py
```

## Run Emilia pipeline directly
```bash
uv run python emilia_pipeline.py --config Emilia/config.json
```

## Required assets
- `Emilia/config.json` (copy from `Emilia/config_example.json`).
- Model files referenced by the config (UVR/MDX and DNSMOS).
- A Hugging Face token for pyannote diarization.

## External tools
- `ffprobe` is used for Higgs export duration metadata. Ensure it is on PATH.

## Output locations
- Projects live under `datasets_folder/`.
- Logs and intermediate outputs are stored under each project folder.
- Emilia pipe writes `<project>_emilia_dataset/` with `audio/` and `<project>_transcribed.jsonl`.
