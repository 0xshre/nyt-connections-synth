# NYT Connections Synthetic Puzzle Generation

## Project Goal

Generate **100-500 high-quality synthetic NYT Connections puzzles** for LLM benchmarking with:
- Full taxonomy coverage (all category types)
- Critical deception/red herrings
- Explicit difficulty control via taxonomy slots
- New category concepts (no overlap with existing 652 puzzles)
- Multi-layered validation (auto + LLM-judge + human review)

---

## Generation Strategy: Category-First with Iterative Deception

### Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    GENERATION PIPELINE                       │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Phase 1: Category Skeleton                                  │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐       │
│  │ YELLOW   │ │ GREEN    │ │ BLUE     │ │ PURPLE   │       │
│  │ Semantic │ │ Assoc/   │ │ Encyc/   │ │ MWE/     │       │
│  │ Relations│ │ Encyc    │ │ Assoc    │ │ Form+Mean│       │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘       │
│       │            │            │            │              │
│       ▼            ▼            ▼            ▼              │
│  Phase 2: Word Generation (4 words each)                    │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐       │
│  │ word1-4  │ │ word5-8  │ │ word9-12 │ │ word13-16│       │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘       │
│       │            │            │            │              │
│       └────────────┴─────┬──────┴────────────┘              │
│                          ▼                                   │
│  Phase 3: Deception Injection                                │
│  ┌─────────────────────────────────────────────────┐        │
│  │ Identify bridge words that fit multiple cats    │        │
│  │ Swap words to create false groupings            │        │
│  │ Verify true solution remains unique             │        │
│  └─────────────────────────────────────────────────┘        │
│                          │                                   │
│                          ▼                                   │
│  Phase 4: Validation Pipeline                                │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐           │
│  │ Auto-check  │ │ LLM-judge   │ │ Human review│           │
│  │ (uniqueness,│ │ (solvable,  │ │ (quality,   │           │
│  │  no overlap)│ │  deceptive) │ │  difficulty)│           │
│  └─────────────┘ └─────────────┘ └─────────────┘           │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Phase 1: Category Skeleton Generation

### Taxonomy Slot Assignment

Each puzzle has 4 slots with assigned taxonomy types:

| Slot | Color | Taxonomy Types | Difficulty |
|------|-------|----------------|------------|
| 1 | Yellow | Semantic Relations (synonymy, hypernymy, co-hyponymy) | Easy |
| 2 | Green | Associative OR Encyclopedic | Medium |
| 3 | Blue | Encyclopedic OR Associative (opposite of slot 2) | Medium-Hard |
| 4 | Purple | MWE, Word Form, OR Word Form + Meaning | Hard |

### Prompt Template for Category Generation

```
You are generating category descriptions for an NYT Connections puzzle.

Generate 4 category descriptions following these taxonomy requirements:

SLOT 1 (Yellow - Easiest): Semantic Relations
- Must be: synonyms, category members (co-hyponyms), or hypernym groups
- Examples: "Ways to walk", "Types of cheese", "Card suits"
- The connection should be immediately obvious

SLOT 2 (Green - Moderate): Associative OR Encyclopedic
- Associative: thematic co-occurrence ("Beach items", "Things that are red")
- Encyclopedic: requires world knowledge ("Oscar winners", "British newspapers")

SLOT 3 (Blue - Challenging): [Opposite of Slot 2]
- If Slot 2 was Associative, this must be Encyclopedic
- If Slot 2 was Encyclopedic, this must be Associative

SLOT 4 (Purple - Hardest): Word Form + Meaning OR MWE
- Word Form + Meaning: "Social media platform endings" (Book, Gram, In, Tube)
- MWE/Fill-in-blank: "B-___" (Ball, Movie, School, Vitamin)
- Wordplay: homophones, palindromes, silent letters

CRITICAL CONSTRAINTS:
- Categories must NOT overlap with existing NYT Connections puzzles
- Categories must be culturally accessible (not too niche)
- Each category must have exactly 4 valid words (no more, no less)

Output format:
{
  "yellow": {"description": "...", "taxonomy_type": "semantic_relation", "subtype": "co-hyponymy"},
  "green": {"description": "...", "taxonomy_type": "associative|encyclopedic", "subtype": "..."},
  "blue": {"description": "...", "taxonomy_type": "encyclopedic|associative", "subtype": "..."},
  "purple": {"description": "...", "taxonomy_type": "mwe|word_form|form_meaning", "subtype": "..."}
}
```

---

## Phase 2: Word Generation

### Prompt Template

```
Given these 4 category descriptions, generate exactly 4 words for each category.

Categories:
{categories_from_phase_1}

Requirements:
1. Each word must ONLY fit its assigned category (no ambiguity within true solution)
2. Words should be common English words (no obscure terms)
3. Prefer single words over phrases (except for MWE patterns)
4. Mix of word types: nouns, verbs, adjectives as appropriate
5. No proper nouns unless the category requires it (e.g., "Directors")

Output format:
{
  "yellow": ["word1", "word2", "word3", "word4"],
  "green": ["word5", "word6", "word7", "word8"],
  "blue": ["word9", "word10", "word11", "word12"],
  "purple": ["word13", "word14", "word15", "word16"]
}
```

---

## Phase 3: Deception Injection

This is the critical phase that makes puzzles challenging.

### Deception Strategies

| Strategy | Description | Example |
|----------|-------------|---------|
| **Polysemy exploitation** | Use words with multiple meanings | WIND (weather vs twist) |
| **False category lures** | Words that seem to form a 5th category | 3 colors + 1 non-color |
| **Cross-category bridges** | Words that plausibly fit 2+ categories | MARS (planet AND red thing) |
| **Semantic neighbors** | Near-synonyms across categories | Similar-sounding actions |

### Prompt Template

```
You have a valid NYT Connections puzzle:
{puzzle_from_phase_2}

Your task: Make this puzzle MORE DECEPTIVE by swapping some words.

Current puzzle analysis:
- Are there any obvious "false categories" that might trap solvers? [analyze]
- Are there words that could plausibly fit multiple categories? [identify]

Deception injection rules:
1. The TRUE SOLUTION must remain valid and unique
2. Add 2-4 "bridge words" that create false groupings
3. Ensure at least one plausible-but-wrong 4-word grouping exists
4. Purple category words should NOT be obviously related to each other

Swap suggestions:
- Replace [word] with [alternative] because [creates false grouping with X, Y, Z]

Output the final deceptive puzzle with explanation of deception vectors.
```

---

## Phase 4: Validation Pipeline

### 4.1 Automated Validation

```python
def auto_validate(puzzle):
    checks = {
        "word_count": len(puzzle.words) == 16,
        "unique_words": len(set(puzzle.words)) == 16,
        "category_size": all(len(cat.words) == 4 for cat in puzzle.categories),
        "no_overlap_existing": not overlaps_with_nyt_dataset(puzzle),
        "common_words": all(is_common_word(w) for w in puzzle.words),
    }
    return all(checks.values()), checks
```

### 4.2 LLM-as-Judge Validation

```
You are evaluating an NYT Connections puzzle for quality.

Puzzle:
{puzzle}

True solution:
{solution}

Evaluate on these criteria (1-5 scale):

1. SOLVABILITY: Can a human reasonably solve this?
   - 1: Impossible/requires obscure knowledge
   - 5: Challenging but fair

2. DECEPTION QUALITY: Are there good red herrings?
   - 1: Categories are obviously separable
   - 5: Multiple plausible false groupings exist

3. CATEGORY CLARITY: Is the true solution unambiguous?
   - 1: Multiple valid solutions exist
   - 5: Only one valid grouping

4. DIFFICULTY CALIBRATION: Does it match intended difficulty?
   - Compare Yellow (should be easy) vs Purple (should be hard)

5. NOVELTY: Is this puzzle creative/fresh?
   - 1: Clichéd categories
   - 5: Creative, surprising connections

Output: {scores, pass/fail, improvement_suggestions}
```

### 4.3 Human Review Interface

For final curation:
- Present puzzle without solution
- Ask reviewer to solve it
- Record time, mistakes, difficulty rating
- Flag puzzles that are unsolvable or too easy

---

## Implementation Files

| File | Purpose |
|------|---------|
| `generator/category_generator.py` | Phase 1: Generate category skeletons |
| `generator/word_generator.py` | Phase 2: Fill categories with words |
| `generator/deception_injector.py` | Phase 3: Add red herrings |
| `validator/auto_validator.py` | Automated checks |
| `validator/llm_judge.py` | LLM-based quality scoring |
| `validator/human_review.py` | Human review interface |
| `data/existing_puzzles.json` | Cache of 652 NYT puzzles for dedup |
| `output/generated_puzzles.json` | Final output |

---

## LLM Configuration

### Provider Strategy: OpenRouter + LiteLLM

Use a model-agnostic approach supporting both:
- **OpenRouter** (proprietary models): For production/quality runs
- **LiteLLM** (self-hosted): For development/cost-sensitive runs

### Implementation

```python
# config.py
from litellm import completion

class LLMConfig:
    def __init__(self, provider: str = "openrouter"):
        self.provider = provider
        self.models = {
            "creative": "anthropic/claude-3.5-sonnet",  # Phase 1, 3
            "precise": "openai/gpt-4o",                 # Phase 2
            "judge": "anthropic/claude-3-opus",         # Phase 4
        }

    def complete(self, prompt: str, task_type: str, **kwargs):
        model = self.models.get(task_type, self.models["creative"])

        if self.provider == "openrouter":
            return completion(
                model=f"openrouter/{model}",
                messages=[{"role": "user", "content": prompt}],
                **kwargs
            )
        else:  # litellm self-hosted
            return completion(
                model=model,
                messages=[{"role": "user", "content": prompt}],
                **kwargs
            )
```

### Model Recommendations by Phase

| Phase | Task | Recommended Model | Reason |
|-------|------|-------------------|--------|
| 1 | Category Generation | Claude 3.5 Sonnet | Creative, follows constraints |
| 2 | Word Generation | GPT-4o | Precise, good vocabulary |
| 3 | Deception Injection | Claude 3.5 Sonnet | Strategic reasoning |
| 4 | LLM-as-Judge | Claude Opus OR GPT-4 | Best judgment quality |

---

## Prompt Engineering Notes

### Temperature Settings
- Phase 1 (Categories): `temperature=0.9` (creative)
- Phase 2 (Words): `temperature=0.3` (precise)
- Phase 3 (Deception): `temperature=0.7` (balanced)
- Phase 4 (Validation): `temperature=0.1` (consistent)

### Few-Shot Examples
Include 2-3 real NYT puzzle examples in prompts to anchor the format and quality expectations.

### Environment Variables

```bash
# .env
OPENROUTER_API_KEY=your_key
LITELLM_API_BASE=http://localhost:8000  # for self-hosted
DEFAULT_PROVIDER=openrouter  # or litellm
```

---

## Quality Metrics Target

| Metric | Target |
|--------|--------|
| Auto-validation pass rate | >95% |
| LLM-judge avg score | >3.5/5 |
| Human solvability | 60-80% |
| Human difficulty correlation | r>0.7 with taxonomy |
| False positive rate (unsolvable) | <5% |

---

## Reference: NYT Connections Taxonomy

See `nyt_connections_study.md` for the full taxonomy study.

### Quick Reference: Category Types

| Type | % | Difficulty | Description |
|------|---|------------|-------------|
| Semantic | 59.6% | Easy | Synonyms, hypernyms, co-hyponyms |
| Encyclopedic | 15.2% | Medium | World knowledge required |
| Word Form + Meaning | 9.6% | Hard | Dual processing |
| Associative | 7.8% | Medium | Thematic co-occurrence |
| Phonology/Morphology | 5.2% | Hard | Sound/spelling patterns |
| MWE | 2.5% | Hardest | Fixed expressions |

---

## Sources

- [arXiv:2412.01621 - NYT-Connections Benchmark Paper](https://arxiv.org/abs/2412.01621)
- [arXiv:2406.11012 - Connecting the Dots: LLM Evaluation](https://arxiv.org/abs/2406.11012)
- [Hugging Face Dataset: tm21cy/NYT-Connections](https://huggingface.co/datasets/tm21cy/NYT-Connections)
- [GitHub: lechmazur/nyt-connections](https://github.com/lechmazur/nyt-connections)
