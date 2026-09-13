# Martina Fuskova, PhD — AI engineer · knowledge graphs · evaluation

**I build systems that show their work, and the instruments that check whether they actually did it.**

Ing., PhD (Mechanical Engineering / Materials, 2026), based in Bratislava. Neo4j GDS certified. Working languages EN/SK.

---

## The through-line

Everything here is one question asked at three levels:

| Level | Question | Where I answer it |
|---|---|---|
| **Engine** | Does it hold up? | [NORMA](https://github.com/TinaFusek/norma) — locating *where* a traversal fails, *why*, and whether the cost is knowable before it runs |
| **Architecture** | Is it built right? | [NORMA](https://github.com/TinaFusek/norma) — tenant isolation, tool surface, super-node modelling, what belongs in a graph and what belongs in object storage |
| **Truth** | Does it tell the truth? | [VERITAS](https://github.com/TinaFusek/veritas) — scoring the reasoning path, not the answer text |

Most RAG evaluation asks whether the answer is correct. That cannot distinguish a correct answer from a lucky one. I make the intermediate representation — the traversal itself — the unit of judgement.

---

## Evaluation

**[VERITAS](https://github.com/TinaFusek/veritas)** — a benchmark that audits the *reasoning path* of GraphRAG systems: path fidelity, provenance coverage, honest-null, overconfidence. Three layers — system trace, adversarial critic, deterministic scorer. The scorer imports no model and no database client, so the same trace plus the same golden gives the same number on any machine.

DOI: [10.5281/zenodo.21919383](https://doi.org/10.5281/zenodo.21919383) · annotation protocol and goldens included · CI on Python 3.10–3.12.

*The finding I care most about:* a model-based critic cannot detect a **structural** coverage gap, because it never sees the schema. Agreement with the deterministic scorer went 77% → 92% → 85% across three critic revisions, and the regression landed precisely on those cases. That is evidence the deterministic layer is not replaceable by a better prompt.

---

## Platform assessment

**[NORMA](https://github.com/TinaFusek/norma)** — can a graph platform safely sit underneath an agent that writes its own queries? Seven pillars, concrete probes against synthetic seeded fixtures, three-state verdicts, and a mandatory evidence level on every finding: measured, documented, or unverified.

Two pillars measured so far, both on Neo4j 2026.05.0:

*A session's default database is a default, not a boundary.* Under database-per-tenant, an identity holding rights to two databases crosses between them with a `USE` clause — one word, 6 ms. A restricted identity is refused on the same route. So isolation is carried by grants, not by the database layout, and a raw query endpoint is incompatible with tenant isolation.

*The obvious cost predictor does not work.* Start-node degree — the cheapest signal available, read from node metadata with zero database accesses — has a 10.6× spread against observed path counts and sometimes ranks nodes backwards. Sum of neighbour degrees drops that to 1.7× at the same ~3 ms cost, holding across an 80× degree range. And the multiplier differs between graph shapes by two to three orders of magnitude, so thresholds calibrate per graph and never transfer.

NORMA grew out of **VITEAL**, a vendor-independent latency benchmark comparing graph engines under identical container limits, replicating a published query set and validating correctness before recording any timing. Its core finding — that deep unbounded traversal fails by materialising paths rather than streaming them, so cost multiplies per level instead of saturating at the reachable node count — is what pillar 3 now measures systematically.

---

## Systems that answer questions

| Project | What it does | Stack |
|---|---|---|
| **[Aurora Compliance](https://github.com/TinaFusek/AuroraCompliance)** | Bilingual EN/SK GraphRAG assistant over the EU AI Act: 554 obligations linked to roles, risk categories, exceptions, deadlines, sanctions. Logs its own traversal as graph nodes, so you can query the agent's own behaviour. | Neo4j, GDS, FastAPI, Claude, D3.js |
| **[MedCheck](https://github.com/TinaFusek/MedCheck)** | Drug-interaction and contraindication assistant over a clinical knowledge graph, with a discrepancy detector that audits the graph itself. | Neo4j, GDS (PageRank, Louvain), Python |
| **[MatGraph](https://github.com/TinaFusek/MatGraph)** | GraphRAG over Materials Project semiconductor data, with provenance-graded edges and a physics interpretation layer. | Neo4j, FastAPI, D3.js |
| **[Athena](https://github.com/TinaFusek/athena)** | Domain-agnostic CV-to-JD scoring with deterministic results — the same input produces the same score, via hash-keyed caching and a rule-based scoring engine. | LangChain, LangGraph, Flask |

---

## Where this came from

**ARTEMISA** — a three-pillar diagnostic system for archival cellulose (pH, FTIR spectroscopy, optical microscopy), built over nearly two years, with a composite Surface Degradation Index fusing GLCM texture entropy, Hough fibre detection, and LAB colour yellowing. MIT GSF grant, real institutional clients.

- *Chemical Condition Assessment of Historical Paper Using Segmented pH Modelling Within the ARTEMISA Framework.* Heritage **9**, 361 (2026). [10.3390/heritage9090361](https://doi.org/10.3390/heritage9090361)
- *Changes in the complex properties of paper cultural heritage objects over time caused by different types of degradation.* Polimery **71**(1), 46–52 (2026). [10.14314/polimery.2026.1.5](https://doi.org/10.14314/polimery.2026.1.5)

It measures a material that cannot tell you it is decaying. Everything above measures a system that cannot tell you it is wrong. Same instinct, different substrate.

---

## What I don't claim

- VERITAS is validated on systems I built. Scoring a system I did not build is the next step, not a completed one.
- Path fidelity was scored on 5 answerable questions; critic agreement on 13. Those numbers never merge.
- NORMA has 2 of 7 pillars measured, on one engine, one configuration, three synthetic fixtures. Applying it to a platform I did not choose is the step that would change its status.
- Cardinality estimation is a mature field with far better estimators than mine. What I measured is that the cheapest naive signal fails, and by how much.
- These systems run locally and have not been operated under sustained production load by a team. That is the part of the craft I am still missing, and I would rather say so than imply otherwise.

I have publicly retracted two of my own benchmark results after finding the errors myself. I would rather do that than defend a number.

---

## Background

Applied photochemistry and materials science (STU Bratislava), BSc in medicinal chemistry. Spectroscopic degradation analysis, DFT tooling, computer vision. Six years in IT talent acquisition before moving into engineering — which is why the HR and compliance domains show up here.

📧 m.fuskova@hotmail.sk · [LinkedIn](https://www.linkedin.com/in/martina-fuskova-phd-601366193/) · [ORCID](https://orcid.org/0009-0006-2418-2517)
