# Martina Fusková — AI engineer · knowledge graphs · evaluation

**I build systems that show their work, and the instruments that check whether they actually did it.**

Ing., PhD (Mechanical Engineering / Materials, 2026), based in Bratislava. Neo4j GDS certified. Working languages EN/SK.

---

## The through-line

Everything here is one question asked at three levels:

| Level | Question | Where I answer it |
|---|---|---|
| **Engine** | Does it hold up? | Vendor-independent latency benchmarking; locating *where* a traversal fails and *why* |
| **Architecture** | Is it built right? | Multi-tenant isolation, super-node modelling, what belongs in a graph and what belongs in object storage |
| **Truth** | Does it tell the truth? | VERITAS — scoring the reasoning path, not the answer text |

Most RAG evaluation asks whether the answer is correct. That cannot distinguish a correct answer from a lucky one. I make the intermediate representation — the traversal itself — the unit of judgement.

---

## Evaluation

| Project | What it does | Stack |
|---|---|---|
| **[VERITAS](https://github.com/TinaFusek/veritas)** | Benchmark that audits the *reasoning path* of GraphRAG systems: path fidelity, provenance coverage, honest-null, overconfidence. Three layers — system trace, adversarial critic, deterministic scorer. The scorer imports no model and no database client. | Python, JSON schemas, pytest, CI on 3.10–3.12 |

DOI: [10.5281/zenodo.21919383](https://doi.org/10.5281/zenodo.21919383) · annotation protocol and goldens included.

The finding I care most about: a model-based critic cannot detect a *structural* coverage gap, because it never sees the schema. Agreement with the deterministic scorer went 77% → 92% → 85% across three critic revisions, and the regression landed precisely on those cases. That is evidence the deterministic layer is not replaceable by a better prompt.

## Systems that answer questions

| Project | What it does | Stack |
|---|---|---|
| **[Aurora Compliance](https://github.com/TinaFusek/AuroraCompliance)** | Bilingual EN/SK GraphRAG assistant over the EU AI Act: 554 obligations linked to roles, risk categories, exceptions, deadlines, sanctions. Logs its own traversal as graph nodes, so you can query the agent's own behaviour. | Neo4j, GDS, FastAPI, Claude, D3.js |
| **[MedCheck](https://github.com/TinaFusek/MedCheck)** | Drug-interaction and contraindication assistant over a clinical knowledge graph, with a discrepancy detector that audits the graph itself. | Neo4j, GDS (PageRank, Louvain), Python |
| **[MatGraph](https://github.com/TinaFusek/MatGraph)** | GraphRAG over Materials Project semiconductor data, with provenance-graded edges and a physics interpretation layer. | Neo4j, FastAPI, D3.js |
| **[Athena](https://github.com/TinaFusek/athena)** | Domain-agnostic CV-to-JD scoring with deterministic results — the same input produces the same score, via hash-keyed caching and a rule-based scoring engine. | LangChain, LangGraph, Flask |

## Engine and architecture

**VITEAL** — vendor-independent traversal and aggregation latency benchmark, comparing graph engines under identical container limits, replicating a published query set and validating correctness before recording any timing.

What came out of it: deep unbounded traversal fails by materialising paths on the heap rather than streaming them, so cost multiplies per level instead of saturating at the reachable node count. The failure is predictable from a single hop — a ~5 ms probe of the start node's frontier — and mitigable by a node-oriented traversal that returns identical counts where the path-oriented one dies. The result is a map of query shape to engine, not a winner.

*Write-up in preparation; repository not yet public.*

---

## Where this came from

**ARTEMISA** — a three-pillar diagnostic system for archival cellulose (pH, FTIR spectroscopy, optical microscopy), built over nearly two years, with a composite Surface Degradation Index fusing GLCM texture entropy, Hough fibre detection, and LAB colour yellowing. MIT GSF grant, real institutional clients, cited internationally (Polymers 2026).

It measures a material that cannot tell you it is decaying. Everything above measures a system that cannot tell you it is wrong. Same instinct, different substrate.

---

## What I don't claim

- VERITAS is validated on systems I built. Scoring a system I did not build is the next step, not a completed one.
- Path fidelity was scored on 5 answerable questions; critic agreement on 13. Those numbers never merge.
- Multi-tenancy findings are verified on a single node. Cloud scaling and distributed consensus are documentation-based analysis, not measurement.
- These systems run locally and have not been operated under sustained production load by a team. That is the part of the craft I am still missing, and I would rather say so than imply otherwise.

I have publicly retracted two of my own benchmark results after finding the errors myself. I would rather do that than defend a number.

---

## Background

Applied photochemistry and materials science (STU Bratislava), BSc in medicinal chemistry. Spectroscopic degradation analysis, DFT tooling, computer vision. Six years in IT talent acquisition before moving into engineering — which is why the HR and compliance domains show up here.

📧 m.fuskova@hotmail.sk · [LinkedIn](https://linkedin.com/in/martina-fuskova)
