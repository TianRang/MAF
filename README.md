# WenYou Bright UI

A lightweight browser-based roleplay chat UI with multi-session conversation management, role card editing, local persistence, markdown rendering, and optional image generation support.

## Features

- Multi-session conversation management
  - Create, switch, delete, clear, import, and export conversations
  - Each conversation stores:
    - messages
    - settings snapshot
    - role card snapshot
- Role card editor
  - Basic info, scenario, creator notes
  - Custom detail entries
  - Worldbook entries
  - Regex rules
  - Import / export role cards as JSON
  - One-click role generation through your configured text model
- Chat
  - Stream and non-stream text generation
  - Local message persistence
  - Markdown rendering
  - Safe inline color tag support:
    - `[color=#7c3aed]text[/color]`
- Image generation
  - Manual image command:
    - `/img your english prompt`
    - `/image your english prompt`
    - `生成图片：your english prompt`
  - Assistant-triggered illustration generation via:
    - `[[IMAGE: ENGLISH_PROMPT]]`
  - Supports:
    - OpenAI-style `/v1/images/generations`
    - NovelAI-style `/ai/generate-image`
- Customizable appearance
  - Accent colors
  - Background colors
  - Card / text / border colors
  - Wallpaper upload or URL
  - Wallpaper opacity
- Fully local persistence with `localStorage`
- Responsive drawer and modal-based UI

## Tech Stack

- Vanilla JavaScript
- HTML
- CSS
- No build tools required

## File Structure
├── index.html
├── styles.css
└── app.js


## How It Works

This is a pure front-end app.

You only need to open `index.html` in a browser, then configure your API settings in the UI.

The app stores all data locally in `localStorage`, including:

- settings
- conversations
- role cards
- wallpaper assets
- generator description drafts

## Quick Start

1. Clone or download this project.
2. Keep these files in the same directory:
   - `index.html`
   - `styles.css`
   - `app.js`
3. Open `index.html` in a modern browser.
4. Click **Settings**.
5. Fill in:
   - Relay API BaseURL
   - API Key
   - text model
6. Optionally configure image generation:
   - image relay base URL
   - image API key
   - image endpoint
   - image model
7. Save settings and start chatting.

## API Compatibility

### Text Chat

The app expects an OpenAI-compatible chat API:

- `GET /v1/models`
- `POST /v1/chat/completions`

Supported modes:

- streaming SSE
- non-stream JSON

### Image Generation

#### OpenAI-style image endpoint

Default endpoint:

- `POST /v1/images/generations`

Expected response can contain either:

- `data[0].url`
- `data[0].b64_json`

#### NovelAI-style image endpoint

Endpoint example:

- `POST /ai/generate-image`

The app can build strict NovelAI-compatible JSON payloads for models like:

- `nai-diffusion-3`
- `nai-diffusion-4-curated-preview`
- `nai-diffusion-4-full`
- `nai-diffusion-4-5-curated`
- `nai-diffusion-4-5-full`

## Conversation Snapshots

Each conversation stores a full snapshot of:

- current role card
- current settings
- message history

When you switch conversations, the app automatically restores:

- API settings
- selected models
- system prompt
- generation parameters
- appearance settings
- role card
- message list

This allows each conversation to behave like an isolated session.

## Role Card Format

Stored role structure:

{
"basic": {
"name": "Night Poet",
"avatar": "",
"playerAvatar": "",
"shortDesc": "",
"persona": "",
"greeting": "",
"talkativeness": 0.4
},
"detail": {
"scenario": "",
"creatorNotes": "",
"custom": []
},
"worldbook": [],
"regex": []
}


The importer also supports a flatter JSON structure such as:

{
"name": "Night Poet",
"shortDesc": "",
"persona": "",
"greeting": "",
"scenario": "",
"creatorNotes": "",
"worldbook": [],
"regex": []
}


## Conversation Import Formats

The conversation importer supports:

1. Full store format

{
"v": 1,
"activeId": "...",
"convs": [...]
}

2. Single conversation format
{
"id": "...",
"title": "...",
"messages": [...]
}

3. Legacy plain message array
[
{ "side": "right", "text": "Hello" },
{ "side": "left", "text": "Hi" }
]


## Image Prompt Rules

For assistant-triggered image generation, the system prompt requires the model to output:

[[IMAGE: <ENGLISH_PROMPT>]]


Important rules:

- must be English only
- must be single-line
- must not contain Chinese characters
- should describe the NPC’s current scene

If Chinese characters are detected in the image prompt, generation will be rejected.

## Local Storage Keys

Main keys used by the app:

- `wy_settings_v1`
- `wy_ui_assets_v1`
- `wy_chat_history_v1`
- `wy_conversations_v1`
- `wy_role_card_v2`
- `wy_role_gen_desc_v1`

## Notes

- This project is front-end only.
- API keys are stored in browser `localStorage`.
- Do not use it on untrusted shared devices.
- Large wallpaper images may consume significant storage space.
- Conversation count is capped.
- Message history per conversation is also capped.

Default limits in code:

- max conversations: `30`
- max stored messages per conversation: `200`

## Browser Requirements

Recommended:

- Chrome
- Edge
- Firefox
- Safari latest versions

Required browser features include:

- `localStorage`
- `fetch`
- `ReadableStream`
- `FileReader`
- `crypto.randomUUID` (fallback exists if unavailable)

## Customization

You can edit defaults in `app.js`, including:

- default settings
- default system prompt
- max conversation count
- max message count
- default colors
- default image size
- default role card

Useful constants:

- `DEFAULT_SETTINGS`
- `DEFAULT_ROLE`
- `CHAT_MAX_ITEMS`
- `CONV_MAX`

## Security Considerations

- Data is stored locally in the browser.
- API keys are not encrypted.
- Markdown rendering is custom and limited, but you should still be careful if you import unknown JSON content.
- This project is intended as a personal/local tool template.

## License

Please add your preferred license here.

## Acknowledgements

Built as a lightweight bright-theme RP chat template with:

- role card editing
- multi-session history
- markdown rendering
- image generation integration
- local-first workflow
