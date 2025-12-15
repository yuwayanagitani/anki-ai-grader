# 🧪⌨️✨ AI Type Grader (Anki Add-on)

**AI Type Grader** automatically grades your **typed answer** in Anki (0–100%) using:
- ✅ OpenAI
- ✅ Gemini
- ✅ Local fallback (string similarity)

It then:
- shows a **score badge** on the back of the card (optional)
- shows a **tooltip** with the score + provider (optional)
- can optionally **override Enter/Space default ease** (Again/Hard/Good/Easy) based on the score

This is designed for “type the answer” workflows where you want more nuance than a simple right/wrong check.

---

## 🌈 What it does

When you reveal the answer in the Reviewer:

1. Reads your **typed answer** (`Reviewer.typedAnswer`)
2. Reads the **correct answer** from a configurable field (default: `Back`)
3. Grades similarity using:
   - AI provider(s), or
   - local similarity fallback if AI fails
4. Displays:
   - a badge on the card (optional)
   - a tooltip (optional)
5. Converts the score into an Anki ease (1–4) using thresholds:
   - Easy / Good / Hard / Again

If enabled, it also patches Anki’s default ease decision so pressing **Enter/Space** can automatically choose the recommended button.

---

## ✅ Supported grading modes

### 1) OpenAI
- Uses the **OpenAI Responses API**
- Sends a short prompt:
  - “Model: … / Student: …”
  - asks for **a number 0–100 only**

### 2) Gemini
- Uses **generateContent**
- Same 0–100 output rule

### 3) Local fallback
If AI fails (network, rate-limit, invalid key, etc.), it falls back to:
- `difflib.SequenceMatcher` similarity ratio → 0–100

---

## 🚀 How to use

1. Use an Anki note type with **typing** enabled (e.g. “type in the answer”)
2. Review as usual
3. When you show the answer:
   - the add-on grades your typed input
   - the score appears (badge and/or tooltip)

Optional:
- With `auto_answer = true`, pressing **Enter/Space** can automatically follow the recommended ease.

---

## 🛠️ Installation

### Option 1: AnkiWeb (recommended)
Install normally via Anki’s add-on workflow and restart Anki.

### Option 2: Manual (GitHub)
Place the add-on folder in your Anki `addons21` folder and restart Anki.

---

## 🔑 API keys

You can store API keys in either:
- the add-on config (recommended for convenience), or
- environment variables (fallback)

### OpenAI
- Config: `openai_api_key`
- Env fallback: `OPENAI_API_KEY`

### Gemini
- Config: `gemini_api_key`
- Env fallback: `GEMINI_API_KEY` or `GOOGLE_API_KEY`

If no key is available, the add-on will skip that provider.

---

## ⚙️ Configuration

Open:
- **Tools → Add-ons → AI Type Grader → Config**

This add-on uses **numbered keys** (e.g. `01. enabled`) to keep config ordering stable.  
It also accepts both numbered and unnumbered keys for backward compatibility.

### General
- `01. enabled`  
  Enable/disable the add-on

- `02. provider`  
  `auto` / `openai` / `gemini` / `local`

- `03. provider_auto_order`  
  When provider = `auto`, try providers in this order  
  Default: `["openai", "gemini"]`

### OpenAI
- `10. openai_api_key` (optional if env var is set)
- `11. openai_model` (default: `gpt-4o-mini`)
- `12. openai_api_url` (default: `https://api.openai.com/v1/responses`)

### Gemini
- `20. gemini_api_key` (optional if env var is set)
- `21. gemini_model` (default: `gemini-2.5-flash-lite`)
- `22. gemini_api_url`  
  Default template:  
  `https://generativelanguage.googleapis.com/v1beta/models/{model}:generateContent`

### Scoring
- `30. answer_field`  
  Field name used as the “correct answer” (default: `Back`)

Thresholds (0–100):
- `31. easy_min` (default: 80)
- `32. good_min` (default: 60)
- `33. hard_min` (default: 40)

Score → Ease mapping:
- score ≥ easy_min → **Easy (4)**
- score ≥ good_min → **Good (3)**
- score ≥ hard_min → **Hard (2)**
- otherwise → **Again (1)**

### UI
- `40. show_tooltip`  
  Show a tooltip with the score/provider

- `41. show_on_card`  
  Show a badge on the **card back**

Badge states:
- “⚙ AI grading…” while pending
- “Score: 87%” with color-coded styling after completion

### Behavior
- `50. auto_answer`  
  If enabled, patches Anki’s default ease so **Enter/Space** can use the AI-recommended ease.

### Networking
- `60. timeout_sec` (default: 20)
- `61. max_output_tokens` (default: 16)  
  (The add-on asks for a number only, so small token limits are enough.)

---

## 🎨 How the badge looks

The add-on injects CSS into the Reviewer WebView and shows a centered badge:

- **Easy**: blue
- **Good**: green
- **Hard**: yellow
- **Again**: red
- **Pending**: gray, dashed border

This display is purely visual and does not modify your notes.

---

## 🧯 Troubleshooting

### “AI grading: field 'Back' not found.”
Your configured `answer_field` does not exist in the note type.  
Fix the field name in settings (Scoring tab).

### Always falling back to “local”
- API keys are missing or invalid
- Provider is set to `local`
- Network issues / rate limits

Check:
- OpenAI key: config or `OPENAI_API_KEY`
- Gemini key: config or `GEMINI_API_KEY` / `GOOGLE_API_KEY`

### “OpenAI HTTP …” / “Gemini HTTP …”
Provider returned an error response. Common causes:
- invalid key
- quota / rate limit
- wrong endpoint URL
- network connectivity

### Enter/Space feels “different”
That’s `auto_answer`. Disable it if you want Anki’s default behavior.

---

## 🔒 Privacy

If AI providers are enabled, your card text is sent to external APIs:
- your typed answer
- the correct answer field content

Do not use with sensitive/private data unless you understand the provider’s policies.

---

## 📜 License

See `LICENSE` in this repository.
