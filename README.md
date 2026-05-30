# GraphDelta

`graph-delta` is a systematic framework designed to bridge **information gain theory** (Google's contextual information gain patent metrics) [1] with the **semantic web** (Wikidata/Wikipedia knowledge graph structures) [2]. 

Traditional SEO focuses on keyword density and mimicking top competitors—a process that often leads to homogenized search results. `graph-delta` reverses this process by identifying:
1. **Core Commodity Topics:** The baseline information you *must* mention to secure relevance.
2. **Information Gain Delta:** Unique data, statistics, and case studies currently missing from competitor pages [1].
3. **Unclaimed Entities:** Contextually relevant adjacent nodes on the Wikidata graph that no competitor has yet mentioned [2].

This tool runs as a strict **State Machine** to ensure deterministic, zero-trust execution.

---

## Architecture Overview