# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

This is a **data repository**, not a code project. It hosts elementary flow lists, flow-mapping files, and supporting documentation maintained by the GLAD Working Group on Nomenclature (WG1) under the UN Environment Life Cycle Initiative. There is no build system, no test suite, no package manager, and no application code to run. "Changes" here mean adding/updating CSV/XLSX data files, mapping outputs, or PDFs — not writing executable code.

## Commands

No build, lint, or test commands exist. Git is the only tooling. Note that `*.xlsx` and `*.xls` are tracked via **Git LFS** (see `.gitattributes`). Install Git LFS before cloning or committing spreadsheets — without it, existing LFS-tracked files check out as small pointer text files (not real spreadsheets), and new commits can bypass LFS and store full binaries directly in Git history. Run `git lfs install` once per machine, and `git lfs pull` on an existing clone to materialize the actual files.

## Repository layout

Four top-level content areas (most have a `README.md`; `Documentation/` has a near-empty `Read me` file):

- **`Formats/`** — canonical schema definitions for the two data artifact types:
  - `FlowList.md` / `FlowList.csv` — schema + empty template for a flow list. Required columns: `Flowable`, `Unit`, `Class`, `Context`, `Flow UUID`. Optional: `CAS No`, `Formula`, `Synonyms`, `External Reference`, `Preferred` (0/1), `AltUnit`, `AltUnitConversionFactor`. Flow list filenames use the list acronym + version (e.g. `IDEAv1.csv`).
  - `FlowMapping.md` / `FlowMapping.csv` — schema + empty template for source↔target mappings. Filenames follow `<Source>to<Target>.csv` (e.g. `IDEAv2.2toFEDEFLv1.0.3.csv`). Required columns cover source list/flow/context/unit, target list/flow/UUID/context/unit; optional `MatchCondition` (`=`, `>`, `<`, `~` meaning equal / superset / subset / proxy, defaults to `=`), `ConversionFactor` (defaults to `1`), plus mapper/verifier provenance fields.
- **`Original Flowlists/`** — pre-harmonization source lists as received (FEDEFL, ILCD EF v2.0/v3.0, ecoinvent EF v3.6). These are the pre-edit baseline and should generally not be modified.
- **`Mapping/`** — the actual mapping workflow:
  - `Mapping/Input/Flowlists/` — cleaned input flow lists (`.csv` + `.xlsx` pairs for FEDEFL, IDEA v2.2 / v2.3, ILCD EF v3.0, ecoinvent EF v3.7).
  - `Mapping/Input/Mapping_files/` — inputs for the JRC mapping tool, named `mapping_input_<SOURCE>_<TARGET>.xlsx`.
  - `Mapping/Output/Mapped_files/` — final mapping outputs, named `<Source>-<Target>.xlsx`. **Per `Mapping/README.md`, the JRC tool output requires manual edits before landing here**; do not treat tool output as the final artifact.
- **`Documentation/`** — reference PDFs (SETAC 2022 presentation, critical review, FEDERAL LCA Commons list, GLAD EF mapping method & issues report v2.2) and an `Images/` folder.

## Conventions for contributions

- When adding or modifying a flow list or mapping file, conform to the schemas in `Formats/` exactly — column order and naming are load-bearing for downstream consumers.
- Preserve the file-naming conventions above; they encode the list/version pairing and are how consumers discover artifacts.
- Maintain the pairing of `.csv` and `.xlsx` under `Mapping/Input/Flowlists/` when updating a list.
- Keep spreadsheets as LFS objects; don't accidentally commit the raw binary or pointer as a regular file.
