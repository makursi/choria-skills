# Changelog

All notable changes to the notes-refiner skill.

## [1.0.0] - 2026-06-09

### Added

- Initial release: 5-phase CS note refinement pipeline
- Domain gating (CS-only with math/CS boundary handling)
- 6-type gap taxonomy: terminology, derivation, algorithmic, context, relationship, application
- Two-tier expansion depth (full Why→How→What for high/medium priority; light for low)
- Flexible confirmation gates (<=8 gaps → 2 prompts; >8 gaps → 3 prompts; auto-proceed mode)
- Bilingual CS terminology (English with 中文 on first use)
- Subfield-specific depth standards (algorithms, OS, networks, databases, AI/ML, PL, distributed systems, cryptography)
- Mermaid concept map generation
- 4-section output document schema
