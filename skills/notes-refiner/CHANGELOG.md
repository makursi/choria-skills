# Changelog

All notable changes to the notes-refiner skill.

## [3.0.2] - 2026-08-28

### Changed

- Frontmatter normalized to collection convention: top-level `version` removed;
  added `metadata` block (author, date-based version `2026.8.28`, source pointing
  to the independent publish repo). Version scheme switches from semver to `YYYY.M.D`.

### Added

- Independent publish repo [makursi/skill-notes-refiner](https://github.com/makursi/skill-notes-refiner):
  publish-channel model — the collection repo remains the single source of truth,
  runtime files are exported one-way for standalone `skills add` installation.

## [3.0.1] - 2026-08-28

### Changed

- Reference files renamed with category prefixes (antfu/skills convention): `workflow.md` → `core-workflow.md`, `output-schema.md` → `core-output-schema.md`, `expansion-guide.md` → `best-practices-expansion.md`
- SKILL.md reference list converted to a categorized table (Core / Best Practices)

## [3.0.0] - 2026-06-11

### Changed (BREAKING)

- **9-element cognitive lens system**: 4-layer framework (资源→抽象→机制→策略) replaced by a
  5-element causal chain (问题→资源→抽象→机制→策略) with 4 embedded analysis layers
  (权衡, 关系, 最小示例, 关键要点). This is a cognitive model, not a template — analysis
  layers are embedded into causal chain sections, not separate headings.
- **问题 (Problem) element added**: new mandatory first element in the causal chain.
  Three-state logic: Full (system/algorithm), Minimal (language/tool, 1 sentence), None
  (pure definition — skip). Operational definition: remove the problem → technology has
  no reason to exist.
- **Output section template**: 4 layers → 5 causal chain sections (visible headings) +
  4 analysis layers (embedded) + optional 验证 (standalone) + mandatory 关键要点 (no heading).
- **Embedding map**: 权衡 → 策略; 关系-is-a → 抽象, 关系-depends-on → 机制; 最小示例 → 机制.
- **Depth assessment**: single L1-L4 scale → dual-layer scoring model (Coverage Score 0-5
  + Depth Bonus 0-5), mapped to L1-L4. Coverage scored per causal chain element; Depth
  Bonus scored per analysis layer.
- **Gap taxonomy**: 6 types → 8 types. Added Type 7 (问题缺口 — Problem Gap) and Type 8
  (权衡缺口 — Trade-off Gap).
- **Phase 2.4 relationship mapping**: 4 edge types → 6 edge types. Added trade-offs-with
  and solves.
- **Phase 2.6 gap analysis table**: added Gap Type column.
- **Phase 3.6 quality checks**: 9 → 15 items, covering all new elements and embedding rules.
- **Subfield depth standards**: rewritten with 问题 layer requirements and 权衡 requirements
  for all 8 subfields.
- **EXAMPLES.md**: all 3 examples rewritten with new framework (TCP handshake, red-black
  tree insertion, OS memory management).

### Added

- 9-element cognitive lens system with fixed causal chain + variable analysis layers
- 问题 (Problem) three-state logic (Full / Minimal / None)
- 权衡 (Trade-offs) as explicit analysis layer embedded in 策略
- 关系 (Relationships) as structured outgoing-edge lists embedded in 抽象 and 机制
- 最小示例 (Minimal Example) as concrete instance walkthrough embedded in 机制
- 关键要点 (Key Takeaways) as mandatory 1-3 sentence summary, no heading, at end of
  every concept
- Dual-layer depth scoring model (Coverage Score + Depth Bonus → L-level)
- Gap types 7 (Problem Gap) and 8 (Trade-off Gap)
- trade-offs-with and solves edge types in Phase 2.4 relationship mapping
- Concept type weighting guide with 9-element columns
- Subfield-Specific Expansion Quick Reference with 9-Element Watch column

### Removed

- 小结 (Summary) heading — replaced by unheaded 关键要点
- "Four layers" terminology throughout all reference files

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
