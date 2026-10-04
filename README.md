# Hi, I'm @oliverfrodo 👋

I'm interested in using my resources to promote creativity and inspiration,
especially through self-hosting and home IoT. I'm currently learning Norsk,
Arduino, and Linux.

## Homelab audio research

My homelab currently uses Storyteller, Voice Clone Studio, and Chatterbox in
SubWAVE. I am evaluating
[`audio.cpp`](https://github.com/0xShug0/audio.cpp) as a shared, native audio
inference layer rather than immediately replacing those user-facing tools.

### Where it could help

- **One reusable backend:** it presents TTS, voice cloning, voice conversion,
  ASR, VAD, diarization, source separation, denoising, and enhancement through
  one C++ runtime, CLI, web UI, and HTTP server.
- **Less environment drift:** its native runtime avoids maintaining a separate
  Python/Conda dependency stack for every audio model.
- **Useful service boundary:** the server provides OpenAI-style speech and
  transcription routes plus generic task routes, making it a plausible shared
  GPU worker behind existing applications.
- **Better resource control:** lazy loading and an LRU-style
  `max_loaded_models` limit can prevent several large, used-once models from
  remaining resident on the GPU.
- **Direct overlap:** its supported families include Chatterbox, so that is the
  best first workload for a like-for-like quality, latency, and VRAM trial.
- **Audio cleanup:** RNNoise, DeepFilterNet2, ZipEnhancer, GTCRN, AudioSR, and
  speech-restoration paths could improve noisy voice references or generated
  output, but enhancement should be measured rather than assumed to improve
  every clip.

### Recommended adoption path

1. Run it as a separate experimental service on the GPU host; do not replace
   the current apps yet.
2. Use one fixed Chatterbox prompt, voice reference, and output format to compare
   the existing SubWAVE path with `audio.cpp`.
3. Record time-to-first-audio, total render time, peak VRAM/RAM, intelligibility,
   speaker similarity, and listening preference across several sentences.
4. Test denoise/enhancement only on copies of source and output audio, retaining
   the originals. Loudness-normalize comparison clips so "louder" is not
   mistaken for "clearer."
5. If results are favorable, put a thin adapter or queue in front of the server
   and migrate one workload at a time. Keep model weights on a shared persistent
   volume and set `max_loaded_models` to match available VRAM.

### Caveats

- This is an inference framework, not automatically a drop-in replacement for
  Storyteller, Voice Clone Studio, or SubWAVE orchestration and UX.
- GPU/backend and streaming support vary by model; CUDA is the project's main
  optimized path.
- Quantization can save VRAM but can also change voice quality or stability, so
  each model and precision needs an A/B check.
- The JSON pipeline feature is experimental. Start with the stable HTTP/CLI
  task surfaces before relying on it for production workflows.
- Model licenses are separate from the framework license; check the license of
  every downloaded checkpoint, especially for public or commercial output.

**Current verdict (October 2026):** strong candidate for consolidating the
compute layer and adding cleanup/transcription capabilities, but validate it as
a backend alongside the existing stack before treating it as a replacement.

## Contact

Use your arms.
