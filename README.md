# Martina Fuskova, PhD — AI engineer · knowledge graphs · evaluation

**I build systems that show their work, and the instruments that check whether they actually did it.**

Ing., PhD (Mechanical Engineering / Materials, 2026), based in Bratislava. Neo4j GDS certified. Working languages EN/SK.

<!-- After signing, add one line here saying what you are doing now, e.g.:
     IT consultant since November 2026, working on network optimisation at customer sites. -->

---

## The through-line

Everything here is one question asked at four levels:

| Level | Question | Where I answer it |
|---|---|---|
| **Engine** | What does a traversal cost, and can you know before you run it? | [NORMA](https://github.com/TinaFusek/norma), pillar 3 |
| **Architecture** | What does the platform *enforce*, as opposed to what it appears to? | [NORMA](https://github.com/TinaFusek/norma), pillar 1 |
| **Truth** | Did it reach the answer by a defensible path? | [VERITAS](https://github.com/TinaFusek/veritas) |
| **Practice** | What do I actually do on day one — and what did I get wrong? | [PRAXIS](https://github.com/TinaFusek/praxis) |

Most RAG evaluation asks whether the answer is correct. That cannot distinguish a correct answer from a lucky one. I make the intermediate representation — the traversal itself — the unit of judgement.

---

## Evaluation

**[VERITAS](https://github.com/TinaFusek/veritas)** — a benchmark that audits the *reasoning path* of GraphRAG systems: path fidelity, provenance coverage, honest-null, overconfidence. Three layers — system trace, adversarial critic, deterministic scorer. The scorer imports no model and no database client, so the same trace plus the same golden gives the same number on any machine.

DOI: [10.5281/zenodo.21919383](https://doi.org/10.5281/zenodo.21919383) · annotation protocol and goldens included · CI on Python 3.10–3.12.

*The finding I care most about:* a model-based critic has no mechanism for detecting a **structural** coverage gap — it never sees the schema. Measured on 13 questions, agreement with the deterministic scorer went 77% → 92% → 85% across three critic revisions, and the regression landed precisely on those cases. The first half of that is an argument from how the layer works; the second is what I observed. Together they are evidence that the deterministic layer is not replaceable by a better prompt.

---

## Platform assessment

**[NORMA](https://github.com/TinaFusek/norma)** — can a graph platform safely sit underneath an agent that writes its own queries? Seven pillars, concrete probes against synthetic seeded fixtures, three-state verdicts, and a mandatory evidence level on every finding: measured, documented, or unverified.

Two pillars measured so far, both on Neo4j 2026.05.0:

*A session's default database is a default, not a boundary.* Under database-per-tenant, an identity holding rights to two databases crosses between them with a `USE` clause — one word, 6 ms. A restricted identity is refused on the same route. So isolation is carried by grants, not by the database layout, and a raw query endpoint is incompatible with tenant isolation.

*The obvious cost predictor does not work.* Start-node degree — the cheapest signal available, read from node metadata with zero database accesses — has a 10.6× spread against observed path counts and sometimes ranks nodes backwards. Sum of neighbour degrees drops that to 1.7× at the same ~3 ms cost, holding across an 80× degree range. And the multiplier differs between graph shapes by two to three orders of magnitude, so thresholds calibrate per graph and never transfer.

NORMA grew out of **VITEAL**, a vendor-independent latency benchmark comparing graph engines under identical container limits, replicating a published query set and validating correctness before recording any timing. Its core finding — that deep unbounded traversal fails by materialising paths rather than streaming them, so cost multiplies per level instead of saturating at the reachable node count — is what pillar 3 now measures systematically.

<!-- VITEAL is the only project named here without a link. Either link the repo,
     or write "(not a public repository)" after the name. -->

---

## Method

**[PRAXIS](https://github.com/TinaFusek/praxis)** — the procedures I actually run, with the code that runs them. Seven runbooks, four scripts. Each runbook is a sequence I have followed, with the mistakes that shaped it named in place.

*What using them taught me:* three defects in one of those scripts were found by running it against a graph I had not seen, and all three were in how it **reported**, not in what it measured — a zero frontier counted as a failed probe, a spread rule that fired regardless of magnitude, and a probe cost quoted as a property of the method when it was the difference between a cold call and a warm one. An instrument can measure correctly and still mislead.

---

## Systems that answer questions

| Project | What it does | Stack |
|---|---|---|
| **[Aurora Compliance](https://github.com/TinaFusek/AuroraCompliance)** | Bilingual EN/SK GraphRAG assistant over the EU AI Act: 554 obligations linked to roles, risk categories, exceptions, deadlines, sanctions. Logs its own traversal as graph nodes, so you can query the agent's own behaviour. | Neo4j, GDS, FastAPI, Claude, D3.js |
| **[CONCORDIA](https://github.com/TinaFusek/concordia)** | Entity resolution on synthetic register data, with every stage measured separately rather than as one end-to-end number, and the data's own ceiling located before any tuning against it. Determinism is checked in CI, because two runs on identical data once produced 5,474 and 5,476 clusters. | Neo4j, Python — standard library only |
| **[MedCheck](https://github.com/TinaFusek/MedCheck)** | Drug-interaction and contraindication assistant over a clinical knowledge graph, with a discrepancy detector that audits the graph itself. | Neo4j, GDS (PageRank, Louvain), Python |
| **[MatGraph](https://github.com/TinaFusek/MatGraph)** | GraphRAG over Materials Project semiconductor data, with provenance-graded edges and a physics interpretation layer. | Neo4j, FastAPI, D3.js |
| **[Athena](https://github.com/TinaFusek/athena)** | Domain-agnostic CV-to-JD scoring with deterministic results — the same input produces the same score, via hash-keyed caching and a rule-based scoring engine. | LangChain, LangGraph, Flask |

<!-- The Aurora row carries a measured number (554 obligations) and the CONCORDIA
     row carries one (5,474 vs 5,476). The other three do not. One measured fact
     per row would bring this table up to the standard of the rest of the page —
     but they have to be your numbers, so I have not invented any. -->

---

## Where this came from

**ARTEMISA** — a three-pillar diagnostic system for archival cellulose (pH, FTIR spectroscopy, optical microscopy), built over nearly two years, with a composite Surface Degradation Index. MIT GSF grant, real institutional clients.

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
