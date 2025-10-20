# Personalized LLM Research for the Blended Shadow Puppet (BSP) Project

### Center for Holistic Integration (CHI)

**Meta-Project:** Balanced Blended Space (BBS) → Blended Shadow Puppet (BSP) / Blended Reality Performance System (BRPS)\
**Researchers:** Kazi Islam • Kazi Tasin\
**Mentor:** Dr. David B. Smith\
**Term:** Fall 2025   |   10 hrs/week each   |   Federal Work-Study placement

---

## Research Protocol & Discussion Suggestions

**Purpose:** establish a shared research rhythm, clarify deliverables, and identify open questions for iterative discussion.\
These points can frame weekly check-ins or lab meetings.

### 🔹 General Protocol

- Follow CHI’s *SRDMPA* cycle → **Speculate → Research → Design → Make → Publish → Assess**.
- Document *every* model run or dataset change in GitHub Issues using the **CHI-Timesheet** template.
- Maintain a shared Zotero library tagged by *Model / Method / Heritage / Evaluation*.
- Use Markdown notebooks (`/notebooks`) for logging experiments; include date, config ID, metrics, and short interpretation.
- Push intermediate artifacts (charts, embeddings, plots) to `/results` weekly.

### 🔹 Discussion Points for the Team

- What defines “personalization” for a model observing puppets—appearance, motion, or symbolic meaning?
- Which visual cues (silhouette, color palette, joint articulation) matter most for cross-heritage recognition?
- How does the notion of *balance* (BBS symmetry) appear when comparing Traditional ↔ BSP trajectories?
- How should we visualize “distance” in heritage space—embedding clusters, temporal animations, or live BRPS projection?
- What ethical framing accompanies cultural remix in computational training data?

### 🔹 Deliverable Hygiene

- Each Phase ends with a *brief report + checklist* before moving forward.
- Use pull requests for substantive code or dataset schema updates.
- Annotate any AI collaboration within commit messages using the tag `#AI-collab`.

---

## 1   Project Overview

This project investigates how **personalized large or vision-language models** perceive and interpret imagery from the **Shadow Puppet World**.\
By comparing **traditional Wayang-inspired puppets**, **BSP-original creations**, and **hybrid reinterpretations**, the research explores how computational agents build internal geometries of similarity, difference, and style.

It directly tests **Balanced Blended Space (BBS)** symmetries:

- *Cognitive ↔ Computational Intelligence* via personalization
- *Mediation Pathway Symmetry* between Image→Text and Image→Image processing
- *Cultural Symmetry* between inherited and emergent creative systems

---

## 2   Objectives

1. Conduct precedent research on recent open-source vision & multimodal LLMs.
2. Build a **heritage-differentiated puppet dataset** (Traditional / BSP / Hybrid).
3. Experiment with multiple **model configurations** (fine-tuned, contrastive, or image-only).
4. Map **visual similarity trajectories**—how style evolves across heritage boundaries.
5. Publish reproducible workflows and visual results within CHI’s Collaborative AI and BRPS frameworks.

---

## 3   Roles and Responsibilities

**Kazi Islam — Model Evaluation & Implementation**

- Benchmark architectures (Qwen2-VL, LLaVA-OneVision, Phi-3.5 Vision …).
- Implement embedding extraction and visualization pipelines.
- Maintain technical notes and Zotero entries.

**Kazi Tasin — Dataset Design & Documentation**

- Curate and tag heritage-stratified image sets.
- Manage JSON metadata schema, ethics records, and repo organization.
- Coordinate reporting, visuals, and publication formatting.

---

## 4   Phase Structure and Deliverables

### **Phase I – Precedent Research (Oct → mid-Nov 2025)**

- Survey 2023-25 open-source VLMs.
- Compare personalization methods (LoRA, QLoRA, PEFT).
- Identify feasible local compute setups.\
  **Deliverables:**
- Precedent Matrix (Spreadsheet + Markdown)
- Annotated Bibliography (Zotero)
- *Open-Source VLM Survey Report v1*

---

### **Phase II – Dataset Design (mid-Nov → Dec 2025)**

#### Heritage Differentiation and Style Taxonomy

| Category         | Description                          | Example Classes                                               |
| ---------------- | ------------------------------------ | ------------------------------------------------------------- |
| **Traditional**  | Classical Wayang Kulit archetypes    | Rama · Sita · Hanuman · Semar                                 |
| **BSP Original** | Modern BSP universe figures          | Aruna · Bayang · Dewi Kirana · Lodra · Gunawan · Nila & Surya |
| **Hybrid**       | Digital/Mechanical reinterpretations | Digital Semar · Mechanical Rahwana                            |

#### Character Metadata Schema (v0.1)

Key blocks: `identity`, `appearance`, `context`, `bbs_mapping`, `heritage`, `data_spec`.\
*(See **`/docs/schema_character.json`** for full example.)*

#### Dataset Architecture

```
/dataset/
    /traditional/
    /bsp/
    /hybrid/
```

#### Vision-Only Track

An alternate dataset path for **image-only representation learning**.\
Each image yields an embedding vector; pairwise distances define *visual similarity trajectories* between heritage categories.

**Deliverables**

- Heritage Dataset (≈150 images + metadata)
- Dataset Design Report v1
- Initial embedding maps (test run)

---

### **Phase III – Experimental Model Configurations (Jan → Mar 2026)**

| Config ID | Mode                          | Input/Output                | Goal                                  |
| --------- | ----------------------------- | --------------------------- | ------------------------------------- |
| **A**     | Text-Paired Fine-Tuning       | Image + Caption → Text      | Personalized semantic descriptions    |
| **B**     | Image-Only Embedding Analysis | Image → Image               | Trajectory of similarity & difference |
| **C**     | Hybrid Contrastive            | Dual Images (T vs BSP)      | Learn style boundaries                |
| **D**     | Few-Shot Adapters             | 10–20 samples per character | Lightweight “personal LLM” modules    |

Each configuration → notebook + evaluation report.

---

### **Phase IV – Evaluation & Visualization (Mar → Apr 2026)**

- Quantitative: Top-K accuracy (text) & intra-class variance (image).
- Qualitative: 2-D/3-D embedding maps (t-SNE / UMAP / PCA).
- Cross-Heritage Trajectory Plots (Traditional → Hybrid → BSP).
- Optional animation or interactive demo in BRPS.\
  **Deliverables:** charts · heatmaps · short video.

---

### **Phase V – Publication (May 2026)**

- Final paper “*Visual Symmetry Trajectories in the BSP World*”.
- Repository release with data and notebooks.
- Integration into BRPS and Collaborative AI meta-projects.

---

## 5   Deliverables Summary

| Phase | Primary Output            | Format               |
| ----- | ------------------------- | -------------------- |
| I     | Survey Report + Matrix    | MD + PDF             |
| II    | Heritage Dataset + Schema | Images + JSON        |
| III   | Four Model Experiments    | Notebooks + Logs     |
| IV    | Trajectory Visuals        | Charts + Video       |
| V     | Final Publication         | Paper + Repo Archive |

---

## 6   Model Configuration Landscape

| Model                       | Strength                          | Use Case | Notes                          |
| --------------------------- | --------------------------------- | -------- | ------------------------------ |
| **Qwen2-VL (7B)**           | Balanced multi-image context      | A / B    | Good local inference           |
| **LLaVA-OneVision (4B/8B)** | Efficient fine-tuning             | A / C    | Ideal LoRA tests               |
| **Phi-3.5 Vision**          | Lightweight · accurate embeddings | B        | Perfect for trajectory mapping |
| **Idefics 2**               | Rich caption generation           | A        | Semantic outputs               |
| **InternVL 2**              | High-fidelity visual encoder      | B / C    | Strong for contrastive tasks   |

---

## 7   Work Hours & Coordination

- 10 hrs/week per researcher; weekly joint meeting with mentor.
- Issues & timecards via CHI Timesheet board.
- Repository → `CHI-CityTech/Personalized-LLM-BSP`.

---

## 8   Ethical & Technical Guidelines

- Use only approved BSP assets or licensed heritage imagery.
- Attribute cultural sources and document lineage in metadata.
- Avoid synthetic alterations that distort cultural signifiers.
- Verify open-source licenses (Apache 2.0 / MIT).
- Report AI collaboration per CHI protocol in every major commit.

---

## 9   Expected Outcomes

- Heritage-balanced visual dataset for future research.
- Comparative study of model personalization strategies.
- Visual trajectory maps showing style evolution Traditional → Hybrid → BSP.
- Contribution to Balanced Blended Space framework testing and CHI’s Collaborative AI ecosystem.

---

### Source Document References

The following internal and supporting materials informed this version:

- *Open Source LLM-Experiment for BBS Puppet Show v1.0* (Kazi Islam, 2025)
- *Open Source LLM Speculation (Phi-3.5-Vision) BBS Project.md* (2025)
- *Open Source LLM Speculation (LLaVA-OneVision-1.5) BBS Project.md* (2025)
- *Open Source LLM Speculation (Qwen2-7B-Instruct) BBS Project.md* (2025)
- *Personal-LLM Meeting Summary – Zoom 2025-10-13.md*
- *DBS – BBS Poster for 22nd Poster Session v2.pdf* (Smith, 2024)

---

*End of Document · v3.1 (Fall 2025 Draft · Revised October 20, 2025)*

