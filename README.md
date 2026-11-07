# SubThai — Android MVP

Mobile MP3/WAV transcription using your own Groq API key, editable Thai translation prompts, local searchable history, audio playback, and SRT/TXT export. Native Java Android UI, blue gradient header and purple buttons.

## Build
GitHub Actions runs on push to main or manually using Actions > Build Android APK > Run workflow. After a successful run, download the SubThai-debug-apk artifact and extract app-debug.apk. This is a test/debug build, not a signed production release. No API key is required to build.

Local requirements: JDK 17, Gradle 8.9, Android SDK 35. Run `gradle :app:assembleDebug`. Gradle wrapper is not bundled; CI installs a pinned Gradle version.

## Use
1. Open Settings and enter your own Groq key. It is encrypted with an Android Keystore AES key.
2. Select translation model and edit the Thai translation prompt. Verify availability and free-tier limits in your own Groq account.
3. Import an MP3/WAV, select source language or auto, and choose whether to translate.
4. Keep the app open while processing. Tap a subtitle card to seek playback, or Edit to change text.
5. Export original, Thai or bilingual SRT/TXT using the Android document picker.

## Honest MVP limitations
- Requires internet for Groq; no offline AI. Audio and text are sent to Groq. Review provider data policies before sending sensitive content.
- Input capped conservatively at 24 MiB. Large-file splitting/resume, background service and batch jobs are not implemented.
- Segment timestamps only; no word-level Thai alignment, diarization, waveform, cue splitting/merging or timing editor yet.
- Translation is batched in groups of 30 cues with stable IDs, response validation and up to 3 retries for HTTP 429/5xx. Timing is retained from source speech.
- Searchable last 100 records stored locally. Imported audio is temporary; persistent access to original audio URI is attempted, so playback may require reimport if the source disappears.
- BYOK direct-to-provider is a deliberate prototype tradeoff, not equivalent to backend key protection. Key is never committed. Android backup disabled.
- Work can stop if the process is killed. Successful transcription is saved before translation, and translation can be retried from the result screen.
- APK build, on-device behavior and live Groq output must be verified; no claim of a successful run before Actions reports it.

## Models
Transcription: whisper-large-v3-turbo. Translation model editable; initial suggestion llama-3.3-70b-versatile. Model availability/rate limits may change. Audio translations endpoint is NOT used for Thai: transcription is followed by text translation.
