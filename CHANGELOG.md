# Changelog

All notable changes to the notes-refiner skill.

## [2.0.0] - 2026-06-11

### Changed (BREAKING)

- **Output structure**: Why → How → What replaced by 资源 (Resource) → 抽象 (Abstraction) → 机制 (Mechanism) → 策略 (Strategy) four-layer framework
- **Code and pseudocode downgraded**: code/pseudocode are now optional verification aids in a separate 验证 (Verification) subsection, not the knowledge body. Light-tier concepts never include verification code
- **Gap taxonomy rewritten**:
  - Type 3: Algorithmic Gap → Formal Specification Gap (形式化缺口): requires preconditions, postconditions, invariants, correctness proof; pseudocode optional verification only
  - Type 4: Context Gap → Motivation Gap (动机缺口 / 资源约束缺口): emphasizes underlying resource constraints that drove the design
  - Type 6: Application Gap → Instantiation Gap (实例化缺口): focuses on real-system instantiations and theory-practice deviations; code optional
- **Subfield hard requirements rewritten**: all "implementation/API/code" references replaced with formal, theoretical, and design-choice requirements
- **General expansion checklist**: removed "algorithm pseudocode" and "concrete code example"; added "formal specification", "resource constraint analysis", "theory-practice gap"
- **Quality heuristics**: expanded from 7 to 8 "good" criteria; code quality checks replaced with formal specification self-consistency and four-layer ordering checks
- **>80% code notes**: Phase 1 now terminates (previously: Phase 3 supplemented with explanations)
- **Skill frontmatter description**: "code examples" → "formal analysis, theoretical models"

### Added

- Four-layer emphasis table (资源/抽象/机制/策略) replacing Why/How/What emphasis table
- 验证代码约定 (Verification Code Conventions) — slim guidelines replacing the full Code Block Conventions decision tree
- Subfield-specific verification code guidance: encouraged for Algorithms/Data Structures, AI/ML, PL; sparing for OS, Networks, Databases, Distributed Systems, Cryptography

### Removed

- Code Block Conventions language selection decision tree (6-entry priority chain)
- All mandatory code/pseudocode requirements from hard subfield standards
- Why → How → What sub-items from Phase 4 topic tree outline

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
