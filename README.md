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
*   **User Action:** Provide your target search query (e.g., `"how to migrate from PostgreSQL to DynamoDB"`).
*   **Engine Action:** Confirms receipt and prepares to search the query.

### State 2: SERP Verification (`SERP_VERIFY`)
*   **Engine Action:** Retrieves the top 10 organic search results (excluding advertisements).
*   **User Action:** Review the URL list. Type `Proceed` to confirm, or paste alternative competitor URLs if you wish to adjust the competitive landscape.

### State 3: Structural Harvesting (`STRUCTURAL_HARVEST`)
*   **Engine Action:** Pulls competitor pages and filters out global elements (navbars, sidebars, ads). It presents the exact `<title>` tags, `<h1>` headers, and 10-word structural quotes for each URL to prove anti-bot bypass.
*   **User Action:** Verify the quotes and type `Proceed`.

### State 4: Delta Analysis (`DELTA_ANALYSIS`)
*   **Engine Action:** Calculates saturation metrics across the competitor corpus and displays the **Information Gain Delta Table** showing which topics are highly saturated and which represent unique opportunities.
*   **User Action:** Type `PROCEED TO ENTITY LINKING`.

### State 5: Keyed Entity Linking (`KEYED_ENTITY_LINKING`)
*   **Engine Action:** Lists current competitor entities and generates **Unclaimed Entities** (relevant concepts missing from all competitors) using a keyed format (e.g., `[E1]`, `[E_MISSING_1]`).
*   **User Action:** Search Wikidata or Wikipedia for each item and provide the URLs in key-value format to prevent list-drift errors (e.g., `E1: https://en.wikipedia.org/wiki/PostgreSQL`, `E_MISSING_1: None`).

### State 6: Target Ingestion (`TARGET_INGEST`)
*   **User Action:** Provide the URL of your existing page to optimize, or type `None` if you are generating a brand new page from scratch.
*   **Engine Action:** Analyzes the target URL (if provided) to measure its current compliance with the core and delta layers.

### State 7: Blueprint Generation (`COMPLETE`)
*   **Engine Action:** Outputs the final **Dual-Layer Content Blueprint** (Baseline + Novelty + Unclaimed Authority) and generates a structured, valid JSON metadata schema ready to be integrated into code or automated CMS mapping pipelines.

---

## Technical Features

*   **Zero-Trust Proofs:** Prevents the system from fabricating competitor data by requiring verbatim contextual heading and paragraph quotes from each source page.
*   **Anti-Shift Keyed Mapping:** Bypasses LLM index-offset limitations. Mapped entities are explicitly keyed, ensuring that user-provided URLs never accidentally bind to the wrong concept.
*   **Relevancy-Novelty Balance:** Ensures you do not build a page solely containing unique information that lacks the core structural keywords required to secure baseline index relevance.