# Calorie Tracker

A small Jupyter notebook project that uses OpenAI's GPT-4o vision model to identify food from a photo and estimate its nutritional information.

## How it works

1. Load a food image (see `images/`) with Pillow.
2. Encode the image to base64 and send it to GPT-4o via the OpenAI Chat Completions API along with a prompt.
3. Two prompting styles are demonstrated:
   - **Free-form recognition** — asks the model to name the food, describe it, and mention typical ingredients/nutritional profile.
   - **Structured nutrition analysis** — asks the model to return a single JSON object with:
     - `food_name`
     - `serving_description`
     - `calories`
     - `fat_grams`
     - `protein_grams`
     - `confidence_level` (`High` / `Medium` / `Low`)
4. Results are rendered inline in the notebook as Markdown.

## Setup

1. Install dependencies:
   ```bash
   pip install openai python-dotenv pillow
   ```
2. Create a `.env` file in this folder with your OpenAI API key:
   ```
   OPENAI_API_KEY=sk-...
   ```
3. Open `calorie_tracker.ipynb` and run the cells top to bottom.

## Project structure

```
calorie_tracker/
├── calorie_tracker.ipynb   # Main notebook: image loading, encoding, prompting, results
├── images/                 # Sample food images used for testing
└── readme.md
```

## Notes

- Uses OpenAI's `gpt-4o` model with vision support.
- The structured prompt is designed to return valid JSON only, making it easy to parse the model's response programmatically for downstream calorie-tracking logic.
- This is an exploratory/learning project — there's no persistence layer or UI yet; estimates come directly from the model's general knowledge rather than a nutrition database.
