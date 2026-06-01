Here is the updated `README.md` containing all the sequential workflow, localization, and entity renaming changes we introduced today.

---

# GraphDelta

`graph-delta` is a systematic framework designed to bridge **information gain theory** (Google's contextual information gain patent metrics) [1] with the **semantic web** (Wikidata/Wikipedia knowledge graph structures) [2]. 

Traditional SEO focuses on keyword density and mimicking top competitors—a process that often leads to homogenized search results. `graph-delta` reverses this process by identifying:
1. **Core Commodity Topics:** The baseline information you *must* mention to secure relevance.
2. **Information Gain Delta:** Unique data, statistics, and case studies currently missing from competitor pages [1].
3. **Unclaimed Entities:** Contextually relevant adjacent nodes on the Wikidata graph that no competitor has yet mentioned [2].

This tool runs as a strict **State Machine** to ensure deterministic, zero-trust execution.

---

## Architecture Overview

[SERP_INIT] ──> [SERP_VERIFY] ──> [STRUCTURAL_HARVEST] ──> [DELTA_ANALYSIS]
│
[COMPLETE] <── [TARGET_INGEST] <── [KEYED_ENTITY_LINKING] <──────┘

The engine operates sequentially through exactly seven states. It will refuse to advance until the completion criteria for the current state are met.

---

## Core Ontology

Every analyzed concept, query theme, or graph node is mapped to one of these standardized classes:
*   `CoreCommodity`: Foundational topic appearing in >70% of competitor documents.
*   `InformationGainOpportunity`: Novel, low-saturation topic appearing in <10% of competitor documents [1].
*   `StructuralGap`: Structured elements (tables, lists, metrics) appearing in 40-70% of pages.
*   `PrimaryEntity`: The core semantic entity under analysis [2].
*   `SupportingEntity`: Contextual concepts represented in the competitor corpus [2].
*   `UnclaimedEntity`: Sibling, parent, or child nodes on the Wikidata graph that are entirely absent from competitor SERP documents [2].
*   `AttributeClaim`: Verbatim statistics, data points, or unique factual claims [1].

---

## Execution Guide

To use `graph-delta`, initialize the prompt in a system-instruction-capable LLM context window. Follow these seven progressive steps:

### State 1: SERP Initialization (`SERP_INIT`)
*   **User Action:** This state is completed across two distinct, sequential conversational turns:
    1. **Turn 1:** Provide your target search query (e.g., `"how to migrate from PostgreSQL to DynamoDB"`).
    2. **Turn 2:** Once prompted, provide your target geographic location or market (e.g., `"UK"`, `"US"`, or `"Global"`).
*   **Engine Action:** Confirms and saves both inputs sequentially before configuring regional search parameters.

### State 2: SERP Verification (`SERP_VERIFY`)
*   **Engine Action:** Retrieves the top 10 organic search results from the localized market verified in State 1 (excluding advertisements) and prefixes them with keys `[U1]` through `[U10]`.
*   **User Action:** 
    *   Type `Proceed` to approve the search results as-is.
    *   **Or** provide a structured override mapping from 1 to 10 using the `[U]` keys to reorder/drop pages, or insert raw URLs to swap in new competitors. (e.g., setting `2. https://new-competitor.com` to replace `U2`, or mapping `3. U2` to shift its position).

### State 3: Structural Harvesting (`STRUCTURAL_HARVEST`)
*   **Engine Action:** Pulls competitor pages and filters out global elements (navbars, sidebars, ads). It presents the exact `<title>` tags, `<h1>` headers, and 10-word structural quotes for each URL to prove anti-bot bypass.
*   **User Action:** Verify the quotes and type `Proceed`.

### State 4: Delta Analysis (`DELTA_ANALYSIS`)
*   **Engine Action:** Calculates saturation metrics across the competitor corpus and displays the **Information Gain Delta Table** showing which topics are highly saturated and which represent unique opportunities.
*   **User Action:** Type `Proceed`.

### State 5: Keyed Entity Linking (`KEYED_ENTITY_LINKING`)
*   **Engine Action:** Lists competitor entities and Unclaimed Entities using unique keys (e.g., `[E1]`, `[E_MISSING_1]`). It outputs a blank, raw template containing all keys.
*   **User Action:** Populate and reply with the template using these exact formats:
    *   `[Key]: [Wikidata URL]` (to bind the URL and keep the current name)
    *   `[Key]: [Wikidata URL] | [Entity Name]` (to bind the URL and override the entity name)
    *   `[Key]: Delete` (to completely scrap/remove the entity from the mapping)
    *   `[ADD_1]: [Wikidata URL] | [Entity Name]` (append this to the bottom of the block to add a completely new entity missed by the engine)

### State 6: Target Ingestion (`TARGET_INGEST`)
*   **User Action:** Provide the URL of your existing page to optimize, or type `None` if you are generating a brand new page from scratch.
*   **Engine Action:** Analyzes the target URL (if provided) to measure its current compliance with the core and delta layers.

### State 7: Blueprint Generation (`COMPLETE`)
*   **Engine Action:** Outputs the final **Dual-Layer Content Blueprint** (Baseline + Novelty + Unclaimed Authority)—incorporating any corrected entity names from State 5—and generates a structured, valid JSON metadata schema ready to be integrated into code or automated CMS mapping pipelines.

---

## Technical Features

*   **Zero-Trust Proofs:** Prevents the system from fabricating competitor data by requiring verbatim contextual heading and paragraph quotes from each source page.
*   **Anti-Shift Keyed Mapping:** Bypasses LLM index-offset limitations. Mapped entities are explicitly keyed, ensuring that user-provided URLs never accidentally bind to the wrong concept.
*   **Inline Entity Correction:** Allows users to refine auto-extracted concept names alongside URL linking in a single step, preventing data-drift in the final knowledge map.
*   **Relevancy-Novelty Balance:** Ensures you do not build a page solely containing unique information that lacks the core structural keywords required to secure baseline index relevance.
