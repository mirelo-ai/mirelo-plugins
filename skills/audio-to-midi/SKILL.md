---
name: audio-to-midi
description: Transcribe an existing audio recording into MIDI, MusicXML, or score notation with Mirelo, and retrieve its transcription notes. Use when the user supplies a recording for transcription; this does not compose music or transcribe speech.
---

# Audio-to-MIDI with Mirelo

Use Mirelo's connected `audio_to_midi` tool and its current schema. This converts an existing recording into symbolic musical artifacts. MIDI, MusicXML, and notation files are outputs, not audio inputs for sound-effect tools.

1. Use the user's existing Mirelo asset reference, or stage their selected recording through `create_upload`. If the client cannot upload, use https://mirelo.ai/studio/mcp-upload and obtain the resulting asset reference.
2. Inspect the recording with `inspect_asset`. Use its measured `duration_ms` for `preflight` with endpoint key `audio-to-midi/v1.0`. Report the estimated credits and time. A request for an estimate does not authorize transcription.
3. Submit only when the user requested transcription and the account can fund it. For metered access, provisioning must be ready with no credit shortfall. Do not change billing settings.
4. Preserve requested tempo and notation options. When the user specifies a fixed tempo, use the schema's fixed-tempo option and that BPM. Otherwise use the tool's detected-tempo defaults.
5. Submit `audio_to_midi` with an idempotency key. Reuse the key only for an identical retry. Track the returned job with `get_job` or `wait_job` until it succeeds or fails; a timeout does not justify another paid submission.
6. Return the available MIDI, MusicXML, and requested notation download links. Prefer the tool's signed download links and treat them as private media capabilities.

When the user asks for note-level analysis, call `get_transcription_notes` on the completed job and follow its cursor only as far as needed for the task. Do not dump all notes when a summary suffices. Only describe the transcription as complete after the job reports success.
