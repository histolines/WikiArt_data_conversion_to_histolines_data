# Bridging Art and History: Bringing WikiArt Data into Histolines

When building comprehensive biographical timelines, connecting cultural artifacts to the moments, locations, and historical figures that shaped them is essential. At [Histolines](https://histolines.com), our mission is to contextualize history by mapping events, people, places, and cultural milestones side-by-side on a unified chronological scale.

Recently, I set out to integrate a dataset of artwork metadata from WikiArt into our core database. To fit seamlessly alongside millions of [historical events and biographical entries](https://histolines.com), raw artwork titles and unstructured text descriptions needed to be systematically parsed, transformed, and enriched using Natural Language Processing (NLP).

Here is an overview of how the pipeline was designed and why this work is critical for digital humanities and historical research.

---

## The Transformation Process

The original dataset contained rich metadata, but key details—such as artwork titles, artist names, dates, and stylistic descriptions—were packed into single, unstructured text strings. The objective was to unpack these strings and translate them into our standardized **Histolines** relational schema (`eventId`, `whoCharName`, `whenYear`, `whereLoc`, `whoRel`, `whatRef`, etc.).

### 1. Structural Parsing

The pipeline first isolates the core metadata components:

* **Creator Identification (`whoCharName`)**: The system scans the caption for the creator delimiter (`"by "`) to extract and standardize the artist's name.
* **Title Extraction (`whoRel`)**: Text preceding the artist identifier is isolated as the artwork's original title.
* **Temporal Mapping (`whenYear`)**: The entry directly following the artist name is evaluated for a standard four-digit year format (`YYYY`). If no year is present, the field is left empty rather than guessing, preserving strict data accuracy.
* **Visual Descriptions (`whatRef`)**: Detailed model-generated image captions (such as Florence-2 visual descriptions) are mapped to the primary reference column to give timeline viewers rich context on what is visually depicted in the artwork.

### 2. Entity Recognition via NLP

Extracting basic text isn't enough to connect an artwork to a broader historical ecosystem. Using spaCy's Named Entity Recognition (NER), the pipeline enriches each entry:

* **Strict Geographic Extraction (`whereLoc`)**: The system scans for geopolitical entities (`GPE` like cities or countries) and physical facilities (`FAC`), capturing the primary location associated with the artwork (such as *"Alberta"* or the *"Bay of Patrae"*).
* **Historical Figure Tagging (`orgCont`)**: Named individuals (`PERSON`) mentioned across the caption are extracted and saved as contextual entities.

### 3. Subject vs. Creator Reconciliation

A major challenge in art history metadata is distinguishing between the **creator** of a piece and the **historical figure depicted** within it (for example, Fra Angelico painting *St. Peter*, or Salvador Dalí depicting a specific individual).

Before exporting the final data, the pipeline performs an automated validation check:

* It compares every person identified by the NLP model against the artist's name (`whoCharName`).
* If a distinct historical person is found in the text, the pipeline dynamically reassigns the roles: the **depicted person** becomes the primary subject in `whoRel`, while the artwork's title is merged into `whatRef`. This ensures that biographical timelines accurately capture when a figure was portrayed in history.

---

## How This Fits into the Histolines Mission

Transforming raw, semi-structured metadata into a clean, machine-readable schema is more than just database maintenance—it directly drives how users explore history on [Histolines](https://histolines.com).

* **Breaking Down Data Silos**: Art history, political history, and scientific progress are often documented in completely separate databases. By converting art datasets into the standard Histolines schema, thousands of artistic masterpieces sit on the exact same chronological continuum as major wars, scientific discoveries, and societal shifts.
* **Uncovering Contextual Connections**: Tagging art entries with proper geographical locations (`whereLoc`) and named entities (`whoRel` / `orgCont`) means users on [Histolines](https://histolines.com) aren't just looking at an isolated painting in a museum—they see *what was happening in that specific city, during that specific year, involving those specific historical figures*.
* **Scalable Humanities Infrastructure**: Automated entity recognition and automated quality control allow us to ingest thousands of records from archives worldwide without losing the structural consistency required for visual timeline rendering.

By continually refining these data transformation workflows, platforms like [Histolines](https://histolines.com) can build a richer, more interconnected visual history of our world.

---

*For the complete implementation details and open-source code, check out the repository files on [GitHub](https://github.com).*
