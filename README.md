# AI Grader for Anki

An add-on that scores typed-answer cards (e.g., “Type in the Answer”) using AI or a local fallback, then shows the result as a badge and/or tooltip.

## Features
- Use AI to evaluate typed answers against the expected response.
- Visual feedback (badge/tooltip) showing score and optional short feedback.
- Configurable scoring thresholds and display options.
- Local fallback option if you prefer not to use cloud AI.

## Requirements
- Anki 2.1+
- For AI scoring: API key for supported provider (OpenAI/Gemini) or configure local model/fallback.

## Installation
1. Clone or download the repository.
2. Place the add-on folder into Anki’s add-ons folder (Tools → Add-ons → Open Add-ons Folder).
3. Restart Anki.

## Configuration
- Set provider and credentials (if using cloud AI).
- Choose scoring granularity, threshold for pass/fail, and feedback verbosity.
- Optionally enable local/simple heuristic fallback (e.g., edit distance).

Example config:
```json
{
  "provider": "openai",
  "api_key_env_var": "OPENAI_API_KEY",
  "show_badge": true,
  "threshold": 0.75
}
```

## Usage
- Works on typed-answer cards; when a user submits an answer the add-on evaluates and displays the score.
- Scores can be logged or shown as tooltips/badges on the card.

## Privacy & Costs
- If using cloud AI, prompts and answers may be sent to the provider. Be mindful of privacy and costs.
- Prefer local fallback for sensitive content.

## Troubleshooting
- If scoring fails, check network, API key, and Anki console for errors.
- If you see unexpected scores, tweak prompt templates and scoring thresholds.

## Development
- PRs and issues welcome. When reporting problems, include Anki version, OS, and sample card setup.

## License
MIT License — see LICENSE file.

## Contact
Author: yuwayanagitani
