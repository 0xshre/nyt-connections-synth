# NYT Connections: A Comprehensive Study

## Overview

NYT Connections is a word puzzle game created by New York Times puzzle editor Wyna Liu. Released in June 2023, it quickly became the second-most-played NYT game after Wordle. The game requires players to group 16 words into 4 categories of 4 words each.

---

## Game Structure

### Basic Mechanics
- **16 words** presented in a 4x4 grid
- **4 categories** to identify (4 words each)
- **4 mistakes** allowed before game ends
- **Color-coded difficulty** revealed upon solving each category

### Difficulty Levels (Color System)

| Color | Difficulty | Characteristics |
|-------|------------|-----------------|
| **Yellow** | Easiest | Straightforward, obvious connections (colors, months, animals, sports teams) |
| **Green** | Moderate | Common knowledge but less immediately apparent |
| **Blue** | Challenging | Nuanced connections, slang terms, less common associations |
| **Purple** | Hardest | Wordplay, homophones, idioms, cultural references, misdirection |

---

## Taxonomy of Knowledge Types (from arXiv:2406.11012)

### Theoretical Foundation

The taxonomy is grounded in cognitive science research on:
- **Word association studies**: How concepts connect via spreading activation
- **Semantic memory organization**: How knowledge is structured in the mind
- **Ad hoc category formation**: Categories that violate typical correlational structure

The framework recognizes that solving Connections requires **three distinct knowledge types** mirroring human cognition:
1. **Lexical knowledge**: Understanding word structure and form
2. **Conceptual knowledge**: Understanding word meanings and relationships
3. **World knowledge**: Understanding real-world facts and entities

---

### The Three Primary Categories

## 1. WORD FORM (5.2% of all categories)

Categories based purely on **linguistic structure** - how words look or sound, independent of meaning.

### 1.1 Phonology/Orthography
**Definition**: Connections based on sound patterns or spelling conventions.

| Example Category | Words | Connection Type |
|-----------------|-------|-----------------|
| "Silent 'W'" | Answer, Two, Wrist, Wrong | Orthographic pattern |
| "Rhymes with..." | Various | Phonological pattern |
| "Contains double letters" | Various | Orthographic pattern |

**Why it's hard**: Requires shifting from meaning-based thinking to pattern recognition on the word surface level.

### 1.2 Morphology
**Definition**: Connections based on word structure - roots, prefixes, suffixes, and affixes.

| Example Category | Words | Connection Type |
|-----------------|-------|-----------------|
| "Noun Suffixes" | Dom, Ion, Ness, Ship | Morphological elements |
| "-TION words" | Nation, Station, Ration, Vacation | Shared suffix |
| "Latin roots" | Various | Etymology-based |

**Why it's hard**: Words are presented in isolation, stripped of their typical context that reveals morphological structure.

### 1.3 Multiword Expressions (MWEs)
**Definition**: Fixed or semi-fixed phrases where the meaning is **non-compositional** (meaning cannot be derived from individual words).

| Example Category | Words | Connection Type |
|-----------------|-------|-----------------|
| "___  break" | Coffee, Spring, Lucky, Commercial | Compound completion |
| "Idioms with COLD" | Feet, Shoulder, Turkey, Blood | Idiomatic phrases |

**Why it's hard**: Requires recognizing that words belong to larger fixed expressions rather than having standalone meanings. The phrase meaning differs from sum of parts.

---

## 2. WORD MEANING (82.6% of all categories)

Categories based on **semantic or conceptual content** - what words mean or represent.

### 2.1 Semantic Relations (59.6% - Most Common)
**Definition**: Classical lexical relationships between word meanings.

| Relation Type | Definition | Example |
|--------------|------------|---------|
| **Synonymy** | Words with same/similar meaning | "Ways to walk": Stroll, Amble, Saunter, Stride |
| **Hypernymy** | Superordinate category membership | "Types of cheese": Brie, Cheddar, Gouda, Swiss |
| **Hyponymy** | Subordinate category membership | "Citrus fruits": Orange, Lemon, Lime, Grapefruit |
| **Co-hyponymy** | Items sharing the same superordinate | "Card suits": Hearts, Diamonds, Clubs, Spades |
| **Homonymy** | Same form, different meanings | "BANK" (river vs financial) |
| **Polysemy** | Related but distinct meanings | "CELL" (biological, battery, prison, spreadsheet) |

**Why LLMs do well**: Pre-training exposes models to massive amounts of synonyms, category memberships, and word relationships.

### 2.2 Associative Relations (7.8%)
**Definition**: Words connected through **thematic context** rather than direct semantic relationship. They co-occur in similar situations but aren't synonyms or category members.

| Example Category | Words | Association Type |
|-----------------|-------|------------------|
| "Things that are red" | Mars, Strawberry, Blood, Fire truck | Color-based thematic |
| "Beach items" | Towel, Sunscreen, Umbrella, Cooler | Situational co-occurrence |
| "Birthday party" | Cake, Candles, Presents, Balloons | Event-based thematic |

**Key distinction from Semantic Relations**:
- Semantic: "Types of fruit" = category membership
- Associative: "Things at a picnic" = thematic co-occurrence (includes fruit, but also blankets, ants, etc.)

**Why it's harder**: Requires understanding contexts where words appear together, not just their dictionary definitions.

### 2.3 Encyclopedic Knowledge (15.2%)
**Definition**: Requires **real-world factual knowledge** about entities, events, or concepts beyond dictionary meanings.

| Example Category | Words | Knowledge Required |
|-----------------|-------|-------------------|
| "British Newspapers" | Globe, Mirror, Post, Sun | Knowledge of UK media |
| "Oscar-winning directors" | Spielberg, Coppola, Scorsese, Eastwood | Film industry knowledge |
| "Chemical elements named after scientists" | Curium, Einsteinium, Fermium, Nobelium | Chemistry + history |

**Why it's hard**: Cannot be solved from word meanings alone - requires external world knowledge.

---

## 3. WORD FORM + WORD MEANING (9.6% of categories)

The **hardest category type** requiring simultaneous understanding of both linguistic structure AND semantic content.

### How Combination Works

The solver must:
1. Recognize a **structural pattern** (form)
2. Understand the **semantic relationship** (meaning)
3. Verify that **both apply** to all four words

### Canonical Example: Social Media Platforms

| Words | Form Analysis | Meaning Analysis |
|-------|--------------|------------------|
| Book, Gram, In, Tube | Word fragments | Social media platforms |

**Solution process**:
1. Form insight: These are parts of compound words
2. Meaning insight: Each completes to a social media platform
3. Combined: Face**BOOK**, Insta**GRAM**, Linked**IN**, You**TUBE**

### Additional Examples

| Category | Words | Form | Meaning |
|----------|-------|------|---------|
| "Tech company parts" | Soft, Micro, Apple, Alpha | Compound fragments | Technology companies |
| "___ Man superhero" | Bat, Spider, Iron, Ant | Compound prefix | Marvel/DC heroes |

**Why it's hardest**:
- Requires **dual processing** - neither form nor meaning alone is sufficient
- Words appear disconnected until both dimensions are recognized
- Often involves cultural/encyclopedic knowledge on top of linguistic analysis

---

## Distribution Statistics (from 1,752 annotated categories)

| Category Type | Count | Percentage | Typical Difficulty |
|--------------|-------|------------|-------------------|
| Semantic Relations | 1,045 | 59.6% | Yellow/Green (Easy) |
| Encyclopedic | 266 | 15.2% | Green/Blue (Medium) |
| Word Form + Meaning | 168 | 9.6% | Blue/Purple (Hard) |
| Associative | 137 | 7.8% | Green/Blue (Medium) |
| Phonology/Orthography/Morphology | 92 | 5.2% | Blue/Purple (Hard) |
| Multiword Expressions | 44 | 2.5% | Purple (Hardest) |

---

## Difficulty-Taxonomy Correlation

### Color Level Distribution by Category Type

```
YELLOW (Easiest)     ████████████████████  Semantic Relations (dominant)
                     ████                   Associative

GREEN (Moderate)     ████████████████       Semantic Relations
                     ████████               Encyclopedic
                     ████                   Associative

BLUE (Challenging)   ████████               Encyclopedic
                     ████████               Associative
                     ████                   Word Form

PURPLE (Hardest)     ████████████████████   Multiword Expressions
                     ████████████████       Word Form + Meaning
                     ████████               Phonology/Orthography
```

### Key Insight: Why Purple is Hard

Purple categories (difficulty 4) typically require:
1. **Non-obvious connections**: Rejecting the first pattern that comes to mind
2. **Linguistic metalinguistics**: Thinking about language structure, not content
3. **Cultural literacy**: Pop culture, idioms, brand names
4. **Dual processing**: Combining form and meaning simultaneously

---

## Annotation Methodology

The taxonomy was developed through:
- **3 linguists** independently annotating 1,752 categories from 438 games
- **Majority voting** for final classification
- **Fleiss Kappa = 0.78** (substantial inter-annotator agreement)
- Categories assigned to **subcategory level** only (not sub-subcategories)

---

## Category Type Examples - Detailed Analysis

### Semantic Categories (Easiest for LLMs)
- **Skin Types**: Normal, Dry, Combination, Oily
  - *Type*: Co-hyponymy (all hyponyms of "skin type")
- **Body hair removal methods**: LASER, PLUCK, THREAD, WAX
  - *Type*: Co-hyponymy with action semantics
- **Twist around**: COIL, SPOOL, WIND, WRAP
  - *Type*: Synonymy (near-synonyms for circular motion)

### Encyclopedic Categories
- **Things made of cells**: HONEYCOMB, ORGANISM, SOLAR PANEL, SPREADSHEET
  - *Type*: Polysemy exploitation + encyclopedic knowledge
  - *Why hard*: "Cell" has 4 different meanings; requires knowing all of them
- **British Newspapers**: GLOBE, MIRROR, POST, SUN
  - *Type*: Pure encyclopedic - these are common nouns that happen to be paper names

### Fill-in-the-Blank (Multiword Expressions)
- **B-___**: BALL, MOVIE, SCHOOL, VITAMIN
  - *Type*: Compound word completion with letter constraint
- **Cold ___**: Brew, Case, Feet, War
  - *Type*: MWE with idiomatic expressions (Cold Feet = nervousness)
- **___ Board**: Skateboard, Scoreboard, Blackboard, Wakeboard
  - *Type*: Compound word completion (suffix pattern)

### Wordplay Categories (Word Form)
- **Silent 'W'**: ANSWER, TWO, WRIST, WRONG
  - *Type*: Phonology/Orthography mismatch
- **Palindromes**: CIVIC, KAYAK, LEVEL, RADAR
  - *Type*: Pure orthographic pattern
- **Homophones of numbers**: WON, TOO, FOR, ATE
  - *Type*: Phonological equivalence to 1, 2, 4, 8

---

## Dataset Structure (Hugging Face: tm21cy/NYT-Connections)

### Schema

```json
{
  "date": "timestamp",
  "contest": "string",
  "words": ["array of 16 words"],
  "answers": [
    {
      "answerDescription": "category name",
      "words": ["array of 4 words"]
    }
  ],
  "difficulty": "float64 (1.3 - 4.8)"
}
```

### Dataset Statistics
- **652 puzzles** total
- **16 words** per puzzle
- **4 categories** per puzzle
- **Difficulty range**: 1.3 to 4.8
- **License**: CC-BY-4.0

---

## Sample Puzzle Analysis

### Puzzle: June 3rd, 2024 (Contest #358)

**Words**: LASER, PLUCK, THREAD, WAX, COIL, SPOOL, WIND, WRAP, HONEYCOMB, ORGANISM, SOLAR PANEL, SPREADSHEET, BALL, MOVIE, SCHOOL, VITAMIN

**Categories**:

| Category | Words | Type | Analysis |
|----------|-------|------|----------|
| REMOVE, AS BODY HAIR | LASER, PLUCK, THREAD, WAX | Semantic | Action verbs with shared purpose |
| TWIST AROUND | COIL, SPOOL, WIND, WRAP | Semantic | Synonyms/near-synonyms |
| THINGS MADE OF CELLS | HONEYCOMB, ORGANISM, SOLAR PANEL, SPREADSHEET | Encyclopedic + Polysemy | "Cell" has 3 different meanings exploited |
| B-___ | BALL, MOVIE, SCHOOL, VITAMIN | Fill-in-blank | Compound words with B prefix |

**Difficulty Rating**: 3.3 (moderate)

**Design Insights**:
- The word WIND is deceptive (could mean weather or the action)
- CELLS category exploits polysemy (biological, solar, spreadsheet)
- B-___ category requires pattern recognition over semantic similarity

---

## LLM Performance Analysis (arXiv:2412.01621)

### Key Findings

| Model | Best Accuracy | vs Human Gap |
|-------|---------------|--------------|
| Claude 3.5 | 40.25% (with hints) | -20.42% |
| GPT-4 | ~30% | -30% |
| Humans | 60.67% | baseline |

### Performance by Category Type (Ranked Best to Worst)

1. **Semantic** - LLMs perform best
2. **Encyclopedic**
3. **Associative**
4. **Morphology/Orthography/Phonology**
5. **Multiword Expressions**
6. **Word Form + Word Meaning** - LLMs perform worst

### Why LLMs Struggle
- The game is designed to penalize "System 1" (quick, intuitive) thinking
- Requires "System 2" (deliberate, analytical) reasoning
- Purple categories specifically exploit LLM weaknesses
- Advanced prompting (CoT, Self-Consistency) shows diminishing returns as difficulty increases

---

## Word Characteristics in Categories

### Deceptive Word Placement
Words are intentionally selected to create false patterns:
- Multiple valid-seeming groupings
- Words that fit multiple categories
- Red herrings designed to mislead

### Word Type Distribution
- **Nouns**: Most common
- **Verbs**: Often in action-based categories
- **Adjectives**: Descriptive categories
- **Proper nouns**: Pop culture, brand names
- **Compound words**: Fill-in-blank puzzles

### Common Category Patterns
1. **Synonym clusters**: Words with similar meanings
2. **Hypernym groups**: Items sharing a superordinate category
3. **Thematic associations**: Conceptually related but different domains
4. **Linguistic patterns**: Rhymes, letter patterns, phonetic similarities
5. **Cultural references**: Pop culture, idioms, slang
6. **Compound completions**: ___ BOARD, B-___, etc.

---

## Sources

- [arXiv:2412.01621 - NYT-Connections Benchmark Paper](https://arxiv.org/abs/2412.01621)
- [arXiv:2406.11012 - Connecting the Dots: LLM Evaluation](https://arxiv.org/abs/2406.11012)
- [Hugging Face Dataset: tm21cy/NYT-Connections](https://huggingface.co/datasets/tm21cy/NYT-Connections)
- [GitHub: lechmazur/nyt-connections](https://github.com/lechmazur/nyt-connections)
- [The Word Finder - NYT Connections Guide](https://www.thewordfinder.com/nyt-connections-clues-and-hints/)
