## Purpose
Document the primary configuration files and the settings used by the UI and pipeline.

## Key configuration files
- `Emilia/config.json`: Required by the Emilia pipe in `gradio_interface.py`.
- `Emilia/config_example.json`: Template you should copy to `Emilia/config.json`.

## Emilia config fields (high level)
- `language.multilingual`: Enables language detection for WhisperX in the Emilia pipe.
- `language.supported`: Allowed language codes.
- `entrypoint.input_folder_path`: Default input folder if not overridden by the UI.
- `entrypoint.SAMPLE_RATE`: Sample rate for preprocessing.
- `separate.step1.model_path`: UVR/MDX separation model path.
- `mos_model.primary_model_path`: DNSMOS scoring model path.
- `huggingface_token`: Required for pyannote diarization.
- `filters`: Segment filtering thresholds (duration, MOS, character count).

## UI-provided overrides
When using `gradio_interface.py`, the UI can override:
- `input_folder_path` (project wavs folder)
- batch size, Whisper model, threads, minimum segment duration
- UVR separation enable/disable
- hash-based naming and processed folder cleanup

The UI also records the Emilia settings used for a run in
`<project>_emilia_dataset/emilia_settings.json` to support resume checks.
