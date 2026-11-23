# SmartChef — Recipe Recommendation System

A command-line recommender that ranks recipes by how well they match the
ingredients you already have, filtered by dietary preference and allergen
exclusions. Also generates a shopping list of what you are missing.

---

## 🧰 Tech stack

| | Role |
| :--- | :--- |
| **pandas** | Loads the recipe dataset for filtering and ranking |
| **Python 3** (stdlib) | Everything else — the CLI, the filters, the shopping list |

One dependency. The matching is set intersection over ingredient lists rather
than anything learned, which is honest about what this is: a scoring function,
not a model.

---

## ▶️ Usage

```bash
# Run with bundled synthetic sample dataset (instant, self-contained)
python "recipe recommender.py" --sample

# Or run with full dataset (if recipes_final.csv is present)
python "recipe recommender.py"
```

If `recipes_final.csv` is not found, the script automatically falls back to
`recipes_sample.csv` so the project is always immediately runnable.

The program asks four things in order, then prints ranked results:

| Prompt | Example |
| :--- | :--- |
| Your name | `Manan` |
| Dietary preference | healthy · comfort food · vegetarian · vegan · high protein · surprise me · no preference |
| Ingredient exclusions | `almond flour, milk` — removes recipes containing an exact match for either ingredient |
| Ingredients you have | `chicken, rice, tomato, onion` |

Results come back with a percentage match. Pick one and it prints the shopping
list — the ingredients from that recipe you did not list.

> ⚠️ Ingredient filtering uses exact matches and **should not be relied on for real
> allergy decisions**. See known limitations.

---

## 🌟 What it does

* 🥗 **Preference filtering** — healthy, comfort food, vegetarian, vegan, high
  protein, or an unfiltered selection.
* 🍅 **Ingredient scoring** — compares your pantry against each recipe's
  ingredient list and computes a percentage match, used to rank results.
* **Ingredient exclusion** — removes exact ingredient matches before scoring.
* 🛒 **Shopping list** — for a chosen recipe, lists the ingredients you do not
  have.
* 🧹 **Dataset preprocessing** — `clean_dataset.py` standardises raw recipe data
  into `recipes_final.csv`.

---

## 📦 Dataset & Sample Data

* **`recipes_sample.csv` (bundled in repo)**: 15 clearly labeled synthetic recipes
  covering varied preferences and allergen combinations. Allows immediate execution
  and testing without downloading large assets.
* **`recipes_final.csv` (about 27 MB, 13,501 recipes in the local copy)**: Full dataset (gitignored due to size).

> ⚠️ **The preprocessing step is not currently reproducible.**
> `clean_dataset.py` reads `recipes_edited.csv`, and that file is not in this
> project. Running the script as-is will fail with a `FileNotFoundError`.
>
> That makes the local `recipes_final.csv` the only copy — **back it up before
> relying on this**, and restore the raw source (or commit it, if it is small
> enough) so the pipeline can be run end to end again.

---

## 📁 Structure

```text
SmartChef Recipe Recommendation System/
├── recipe recommender.py   # Interactive CLI recommender (supports --sample)
├── clean_dataset.py        # Preprocessing → recipes_final.csv
├── recipes_sample.csv      # Bundled synthetic sample dataset (15 recipes)
├── recipes_final.csv       # Cleaned full dataset (~27 MB, gitignored)
├── requirements.txt
└── readme.md
```

---

## 🔧 Setup

```bash
cd "SmartChef Recipe Recommendation System"
pip install -r requirements.txt
python "recipe recommender.py"
```

Without `recipes_final.csv`, the command uses the bundled synthetic sample.

**Dependencies:** pandas.

The program asks for your name, preference, allergies and available ingredients,
then prints ranked recommendations with match percentages.

---

## ⚠️ Known limitations

- **Matching is string-based.** `"tomato"` and `"tomatoes"` are treated as
  different ingredients, and `"chicken breast"` will not match `"chicken"`.
  Stemming, or an ingredient synonym table, would fix most of this.
- **Exclusions compare exact ingredient names.** A `nuts` filter will not catch
  `almond flour` or `marzipan`. **Do not rely on the recommender for allergy decisions.**
- **The whole CSV is loaded into memory** on every run — fine at 27 MB, wasteful
  beyond that.
- **No persistence.** Preferences are re-entered every session.
- **Scoring ignores quantities** — having one of ten ingredients counts the same
  per-item as having nine.

---

## 👨‍💻 Author

**Manan Arora**
