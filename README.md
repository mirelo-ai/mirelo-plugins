# Mirelo plugins for Cursor and Claude

Generate sound effects, foley, and ambience from text or video, edit existing audio, and transcribe recordings into MIDI and notation. This plugin pairs Mirelo's hosted MCP connector with two workflow skills: sound design and Audio-to-MIDI.

## Requirements

Use a Mirelo Studio account and sign in through Mirelo OAuth when connecting. Connecting is free. Generation, editing, and transcription use your existing Studio credits; no separate MCP subscription or API key is required. Mirelo's service is intended for adults, as specified in its [Terms of Service](https://mirelo.ai/terms).

## Install and connect

This repository packages the same Mirelo integration for Cursor and Claude. Each platform has its own manifest and MCP configuration; the workflow skills, assets, license, and service documentation are shared. GitHub redirects the earlier Cursor repository URL to this repository, preserving existing installation links.

These packages are being prepared for Marketplace and Directory review and are not yet listed.

### Cursor

Add `https://github.com/mirelo-ai/mirelo-plugins` as a plugin marketplace in Cursor and select Mirelo. For local testing, place this folder under `~/.cursor/plugins/local/mirelo` and reload Cursor; local plugin imports must be allowed by your workspace. Connect the Mirelo MCP server and sign in through Mirelo OAuth.

Alternatively, add the contents of `mcp.json` to your Cursor MCP configuration, preserving other server entries. Manual configuration tests the server connection; it does not verify Marketplace installation.

### Claude

For testing in Claude, zip this repository's plugin files, including the hidden `.claude-plugin` folder and `.mcp.json`, and upload the archive through Customize > Plugins > Add > Upload plugin. Open the plugin's Connectors tab, connect Mirelo, and complete Sign in with Mirelo. On Team or Enterprise, an Owner may need to add the connector for the organization first.

For Claude Code, load this folder with `claude --plugin-dir /absolute/path/to/mirelo-plugins`, then use `/mcp` to connect. The skills are available as `/mirelo-sound-design:sound-design` and `/mirelo-sound-design:audio-to-midi`.

The remote MCP URL is `https://mcp.mirelo.ai/mcp`.

## Example prompts

- Check my Mirelo credits and estimate a four-second glass-breaking sound effect. Do not generate it yet.
- Generate footsteps and impact sounds for the first four seconds of my gameplay clip.
- Extend this ambience by two seconds, preserving the existing audio.
- Replace only the section from one to two seconds with distant thunder.
- Convert this existing melody recording into MIDI and notation.

The skills use preflight estimates before paid jobs. Requests return asynchronous job references; poll the existing job until it finishes rather than submitting again. Retries of the same logical request reuse an idempotency key.

## Scope

The connector supports account and credit checks, cost estimates, uploads, media inspection, text-to-SFX and video-to-SFX generation, audio extension, selected-region inpainting, Audio-to-MIDI, job status, and transcription note retrieval. This integration does not compose music or provide speech-to-text, captions, or video timeline editing. Audio-to-MIDI transcribes an existing recording; it does not compose new music.

## Repository layout

- `.cursor-plugin/plugin.json` and `.cursor-plugin/marketplace.json`: Cursor plugin and repository installation metadata.
- `mcp.json`: Cursor MCP connection.
- `.claude-plugin/plugin.json`: Claude plugin metadata.
- `.mcp.json`: Claude MCP connection.
- `skills/`: shared sound design and Audio-to-MIDI workflows.
- `assets/`, `LICENSE`, and `NOTICE`: shared branding and licensing.

## Files and data handling

This package contains JSON connection metadata, Markdown instructions, and a brand image. It has no hooks, executable scripts, package dependencies, or bundled server. It does not scan local files or collect entire conversations. The skills send only the selected inputs and media needed for the user's requested operation.

MCP calls go to `https://mcp.mirelo.ai/mcp`. Mirelo authenticates the user's account, processes prompts and uploaded media, stores assets and job records, and returns credit/provisioning status, media metadata, job references, optional transcription notes, and temporary upload/download links to the connected client. User-selected media can contain personal information. Treat media access links as private.

For local media, `create_upload` returns a private storage upload URL. A client capable of uploading can send the selected file directly to Mirelo's Amazon S3 storage using that URL; these bytes do not pass through the conversation. Downloading results can likewise reach Mirelo's S3 storage through a signed link. These transfers are part of the requested workflow and occur outside the MCP transport. If the client cannot upload, use [Mirelo's browser uploader](https://mirelo.ai/studio/mcp-upload) and pass the resulting asset reference. Do not put large base64 files into chat or assume that an arbitrary external URL is an accepted tool input.

## Privacy policy

[Mirelo's Privacy Policy](https://mirelo.ai/privacy) governs hosted service processing, retention, deletion, service providers, and any use of content to improve models. The package itself has no separate database or telemetry. The policy does not promise a fixed deletion interval; data is deleted when its processing purpose no longer applies or it is no longer required, subject to applicable obligations. Contact `legal@mirelo.ai` for privacy questions and opt-out requests.

## Troubleshooting and support

If the tools are unavailable, check that Mirelo is connected and signed in through your client. If an upload fails, use the browser uploader. If a paid request cannot proceed, report the preflight's provisioning status or credit shortfall without repeatedly submitting the job. For a running job, continue checking the original job reference.

- Setup documentation: https://mirelo.ai/mcp
- Support: https://mirelo.ai/support or `support@mirelo.ai`
- Service terms: https://mirelo.ai/terms

## License

Configuration and documentation use the [MIT License](LICENSE). Mirelo logos and branding are excluded; see [NOTICE](NOTICE). This package does not grant rights to Mirelo trademarks or change the hosted service terms.
