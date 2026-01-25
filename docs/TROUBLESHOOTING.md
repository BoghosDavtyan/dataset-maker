## Emilia config not found
Symptom: Gradio error "Emilia configuration not found".
Fix: Copy `Emilia/config_example.json` to `Emilia/config.json` and update paths/token.

## Hugging Face token errors (pyannote)
Symptom: diarization pipeline fails on startup.
Fix: Set `huggingface_token` in `Emilia/config.json` to a valid token.

## WhisperX pyannote 4.0 incompatibility
Symptom: transcription fails with `Inference.__init__() got an unexpected keyword argument 'use_auth_token'`.
Fix: WhisperX is hardcoded to use Silero VAD in this repo to avoid pyannote <4.0 API expectations.

## Qwen 3 TTS reference audio list is empty or wrong
Symptom: the Qwen 3 TTS reference audio dropdown only shows files from `wavs/` or is empty.
Fix: ensure `transcribe/` contains audio files; the picker is sourced from `transcribe/`, not `wavs/`.
Note: selecting the Qwen 3 TTS export format refreshes the dropdown automatically.

## Onnx runtime CUDA provider missing
Symptom: Only CPU provider is available.
Fix: Reinstall `optimum[onnxruntime-gpu]` in the uv environment (see `README.md`).

## ffprobe not found (Higgs export)
Symptom: Higgs export duration is 0 or errors mention ffprobe.
Fix: Install FFmpeg and ensure `ffprobe` is on PATH.

## LLM reformatter model missing
Symptom: `llm_reformatter_script.py` cannot load the model file.
Fix: Place the `DeepSeek-R1-Distill-Qwen-32B-Q4_K_M.gguf` model in the working directory
and update the path in `llm_reformatter_script.py` if needed.
