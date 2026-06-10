---
name: understand
description: Use when running codebase orientation, analyzing project dependencies, building a local knowledge graph, or performing incremental updates using Egonex-AI/Understand-Anything.
---

# Codebase Orientation (Understand-Anything)

Analyze codebases, build interactive knowledge graphs, and perform token-efficient, incremental updates using Egonex-AI's `Understand-Anything` tool suite.

## Core Commands

| Command | Action |
| :--- | :--- |
| `/understand` | Runs full codebase scan and AST parser to build local knowledge graph. |
| `/understand-dashboard` | Opens interactive web UI dashboard to explore the dependency graph. |
| `/understand-chat <query>` | Asks specific structural or semantic questions about code design. |
| `/understand-diff` | Performs impact analysis on uncommitted local git changes. |

---

## 🔄 Analysis & Merge Phases

When executing or updating the knowledge graph, follow these sequential phases:

```mermaid
graph TD
    A([Start Scan]) --> B[Phase 1: AST Parsing & File Scanning]
    B --> C[Phase 2: Generate Node & Edge List]
    C --> D{Is Incremental Update?}
    D -- Yes --> E[Phase 3: Write changes to batch-0.json]
    D -- No --> F[Phase 4: Run merge-batch-graphs.py]
    E --> F
    F --> G[Phase 5: Cleanup temporary scratch files]
    G --> H([Graph Updated & Saved])

    style A fill:#4F46E5,stroke:#312E81,stroke-width:2px,color:#fff
    style D fill:#F59E0B,stroke:#78350F,stroke-width:2px,color:#fff
    style E fill:#10B981,stroke:#065F46,stroke-width:2px,color:#fff
    style H fill:#4F46E5,stroke:#312E81,stroke-width:2px,color:#fff
```

### Phase 1: AST Parsing
- The tool scans all files in the workspace (excluding `.gitignore` patterns).
- Resolves import/export relationships, function calls, class hierarchies, and type dependencies.

### Phase 2: Node & Edge List Generation
- Generates JSON node descriptors for all classes, functions, and modules.
- Generates edge descriptors representing structural calls or imports.

### Phase 3: Incremental Update (Bugfix Protocol)
> [!IMPORTANT]
> When updating an existing graph, the tool merges new nodes with the base graph.
> *   **Do NOT** write changes to `batch-existing.json`. The merge script (`merge-batch-graphs.py`) uses a regular expression that only processes files containing digits (e.g., `batch-<N>.json`).
> *   **Correct action:** Always write updated/pruned nodes to a numerically indexed file, such as `batch-0.json` (or `batch-1.json`, etc.), so that the merge script correctly picks it up.

### Phase 4: Merging
- Combines the newly scanned changes with the existing graph metadata.
- Prunes orphaned nodes (e.g., deleted or renamed files) to maintain graph hygiene.

### Phase 5: Cleanup
- Safely remove temporary build folders and intermediate JSON chunks.
- Save the final output to `.understand-anything/knowledge-graph.json` and generate `ONBOARDING.md` in the project root.
