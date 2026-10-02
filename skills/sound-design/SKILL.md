---
name: sound-design
description: Generate or edit sound effects, foley, or ambience with Mirelo, including sounds synchronized to a supplied video, audio extension, and selected-region repair. Use for a requested Mirelo sound design workflow; music composition and speech transcription are outside this skill.
---

# Sound design with Mirelo

Use the connected Mirelo MCP tools for the requested sound design task. Read their current input schemas and limits rather than assuming fixed model versions or duration bounds. If Mirelo is unavailable, explain how to connect it from the client's plugin or MCP settings.

## Choose the operation

- Text description of a sound or ambience: `text_to_sfx`.
- Sounds synchronized to a supplied video: `video_to_sfx`. Omit its optional prompt unless the user has given an explicit sound instruction; the video can drive the result itself.
- Add audio after an existing clip: `extend_audio`, or `extend_audio_with_video` when the user supplies video guidance. Prefer `append_duration_ms` for the requested addition; provide exactly one of that and total `duration_ms`.
- Replace a specified time region: `inpaint_audio`. Keep the requested start and end boundaries, converted to milliseconds, and preserve the rest of the clip.

Music composition is unavailable on this MCP surface. Do not approximate it with a paid sound-effect request. For MIDI or notation from an existing recording, use the Audio-to-MIDI skill.

## Prepare media

Use an existing Mirelo `asset_id` when supplied. For a local file the user selected, use `create_upload` and transfer that file to its returned upload URL when the client supports uploads. Do not upload until the file choice and intended operation are clear. If the client cannot perform the transfer, direct the user to https://mirelo.ai/studio/mcp-upload and use the resulting asset reference.

Call `inspect_asset` when input metadata is needed for the requested window or cost estimate. Do not invent a duration when it cannot be measured. Hosted tools accept Mirelo download links as URL inputs; arbitrary third-party URLs and raw result storage URLs need downloading and staging as a Mirelo asset first. Avoid large inline base64 uploads.

## Estimate and submit

Call `preflight` with the operation's advertised endpoint key, duration, sample count, and loop setting. Report the estimated credits and time. A request to estimate or inspect does not authorize generation. When generation is already requested and its parameters are clear, continue without an extra confirmation unless the client requires one.

For metered requests, proceed only when provisioning is ready and there is no credit shortfall. Report a blocked request without recommending purchases or changing billing settings. Respect unmetered account access when the tool reports it.

Give each new logical job an idempotency key. Reuse it only when retrying the same tool and parameters, so a transport failure does not create another paid job. A changed request needs a new key.

## Finish the job

Retain the returned `job_id`. Prefer `get_job` when the client can poll; use `wait_job` when a blocking wait is appropriate. A timeout is a reason to check the existing job, not submit a duplicate. Do not describe a result as generated until the job succeeds.

Return a playable or downloadable result with its actual duration and a short description. Prefer `download_urls`; use `result_urls` as a download fallback. Temporary links are private media capabilities. Keep them out of public logs or shared documents. If the user asks for a follow-up edit, use the completed result as the input and submit a new job for that edit.
