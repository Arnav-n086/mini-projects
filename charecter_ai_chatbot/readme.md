# Character AI Chatbot

A small Jupyter notebook that uses the OpenAI Chat Completions API to roleplay as different fictional characters via system prompts.

## What it does

1. Connects to the OpenAI API using an API key loaded from a `.env` file.
2. Sends a plain test message (no persona) to confirm the client works.
3. Defines a `character_personalities` dictionary mapping character names to detailed system prompts describing their personality, speech style, and catchphrases. Included characters:
   - **Sherlock Holmes** — analytical, formal Victorian detective
   - **Tony Stark** — witty, sarcastic genius billionaire
   - **Yoda** — wise, speaks in inverted syntax
   - **Hermione Granger** — precise, well-read Hogwarts student
   - **Fang Yuan** — calm, ruthlessly pragmatic immortal cultivator (Reverend Insanity)
4. Picks a character via the `chosen_character` variable and uses its system prompt as the `system` message in a chat completion, alongside a user message, so the model replies in that character's voice.

## Requirements

- Python packages: `openai`, `python-dotenv`
- A `.env` file in the project directory containing:
  ```
  OPENAI_API_KEY=your-api-key-here
  ```

## Usage

Run the notebook cells in order. To talk to a different character, change the `chosen_character` variable in the persona-selection cell to any key in `character_personalities` (or add your own), then re-run the final cell with a new `user_first_message`.
