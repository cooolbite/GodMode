# Release Notes

## v6.0.0 — Initial Release

This release marks the first official version of the project fork. It resets the release history and focuses on delivering a zero-dependency, local-first skills framework for agentic software development workflows.

### Highlights

*   **New Agent Collaboration Skill (`agent-collaboration`):** A state-of-the-art methodology for multi-agent workflows, drawing inspiration from CrewAI, LangGraph, and AutoGen.
    *   **Role-Based Specialized Teams:** Establishes clear roles (Planner, Implementer, Verifier) with distinct tool bounds.
    *   **Structured Handoff State:** Replaces open-ended text handoffs with structured, validated state exchange.
    *   **Circuit Breakers:** Implements a loop threshold detector to prevent infinite correction loops.
*   **Simplified Local Setup:** Removed complex plugin marketplace configurations. Users can now easily clone the repository and place the `skills/` folder directly in their respective agent's config folder (e.g., `~/.claude/skills/` for Claude Code).
*   **Methodology Preservation:** Retains core tested practices including Test-Driven Development (TDD) via red-green-refactor, systematic debugging, Socratic brainstorming, and subagent planning.
