# Mirelo for Cursor

Connect Cursor to Mirelo's hosted MCP server to generate and edit sound effects, foley, and ambience, or convert existing recordings into MIDI and notation.

## Requirements

A Mirelo Studio account with MCP access and sufficient credits for paid jobs. Sign in through Mirelo OAuth when prompted by Cursor. No API key belongs in this plugin or your MCP configuration.

## Installation

This package is being prepared for Marketplace review and is not yet listed.

For local plugin testing, place this folder under `~/.cursor/plugins/local/mirelo` and reload Cursor. Local plugin imports must be allowed by your workspace. Open Customize and connect the Mirelo MCP server.

Alternatively, add the contents of `mcp.json` to your Cursor MCP configuration, preserving other server entries. Manual configuration tests the server connection; it does not verify Marketplace installation.

## Example prompts

- Check my Mirelo credits and estimate one 4-second rain sound effect. Ask before generating it.
- Generate foley for the first four seconds of this video after inspecting it and estimating the cost.
- Extend my last sound effect by two seconds after estimating the cost.
- Replace only the section from one to two seconds with distant thunder.
- Convert this existing melody recording into MIDI and notation.

Use preflight estimates before paid jobs. Generation and transcription consume Mirelo credits. Jobs are asynchronous: wait for the existing job or poll its status before retrieving results. Keep the same idempotency key when retrying a submission.

For browser uploads, use https://mirelo.ai/studio/mcp-upload and supply the resulting asset reference. Generated download links are temporary; retrieve results before they expire. Treat media links as private access links.

## Scope and data

Tools support sound-effect generation, audio extension with or without video context, selected-region inpainting, account/credit checks, media inspection, and Audio-to-MIDI transcription. Music composition, speech-to-text, and video timeline/caption editing are outside this MCP surface.

Cursor sends selected tool inputs and media references to Mirelo. Tool results can include credit status, media metadata, job references, note data, and temporary upload/download links. Consult the policies below for service data handling.

- [Mirelo MCP](https://mirelo.ai/mcp)
- [Support](https://mirelo.ai/support)
- [Privacy](https://mirelo.ai/privacy)
- [Terms](https://mirelo.ai/terms)

The package contains connection metadata and documentation. The hosted service remains operated by Mirelo. This package does not grant rights to Mirelo trademarks or modify service terms.

## License

Configuration and documentation are licensed under the [MIT License](LICENSE). Mirelo logos and branding are excluded; see [NOTICE](NOTICE).
