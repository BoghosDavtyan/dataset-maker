# Author Notes

## Purpose
Author-facing constraints and preferences that must be applied when updating or
verifying documentation.

## Notes
- The Emilia pipe was created with an intent to be used for IndexTTS 2, but it is not exclusive to IndexTTS 2; it is a general pipeline for building the Emilia dataset.
- WhisperX is hardcoded to use Silero VAD because the repo uses pyannote 4.0, while WhisperX expects <4.0 (the `use_auth_token` path breaks).
