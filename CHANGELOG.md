# Changelog

## 2026-01-24
- Inconsistencies found: README Qwen 3 TTS example implied fixed `sample_000000.wav` naming; `docs/ARCHITECTURE.md` export flow omitted Qwen 3 TTS.
- Documentation updates: clarified Qwen 3 TTS audio naming in `README.md`; added Qwen 3 TTS to export flow in `docs/ARCHITECTURE.md`.
- Added Qwen 3 TTS export with reference audio selection and dataset JSONL generation.
- Changed Qwen 3 TTS export to use `transcribe/` for reference audio, skip missing transcripts, and store audio under `data/` with updated JSONL paths.
- Switched WhisperX VAD to Silero for pyannote 4.0 compatibility and documented the reason.
- Updated transcription segment naming to `seg_00000001` style for stable lexicographic ordering.
- Updated Qwen 3 TTS UI to refresh the reference audio list when the export format is selected.
- Documented the Qwen 3 TTS dropdown auto-refresh behavior.
