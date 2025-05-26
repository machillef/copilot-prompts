# Resume Scanner Web App – Phase 1 PRD

*copilot-instructions.md*
**Last updated:** 2025‑05‑24

---

## 1. Objective

Build a **minimal, non‑containerised** web service that lets a user upload a résumé (`.pdf`, `.doc`, `.docx`, ≤ 50 MB). The service must:

1. Parse the file using an Applicant‑Tracking‑System‑style (ATS) extractor.
2. Highlight parsing issues (fields missed, suspect dates, unusual formatting, etc.).

This PRD **is the single source of truth** for GitHub Copilot or any LLM generating the Phase 1 code base. All requirements below are mandatory unless marked *optional*.

---

## 2. Scope

### 2.1 In‑Scope (Phase 1)

* Upload résumé via web UI and REST endpoint.
* File validation (size, type, duplicate suppression).
* Python‑based parsing pipeline producing structured JSON.
* Minimal UI that renders:

  * Parse summary (name, headline, experience timeline, skills list, etc.).
  * Issue list (missing fields, suspicious gaps, unreadable sections).
* It's mandatory that *nothing* gets saved in the application or server. Everything should be stored in memory.
* All user data, uploaded resumes and summary/issues should not be stored. Everything should be anonymous and transient.
* Keep nothing for storage.
* Always try and implement big changes in batches and test the application before moving forward. Its essential to not break existing functionality
* Use `FastAPI` for the web service, with typed endpoints and auto‑generated OpenAPI docs.
* Always git commit after completing a task or feature, with a clear message.
* Simple unit tests for the parsing logic and web endpoints.

### 2.2 Out‑Of‑Scope (Phase 1)

* **Containerisation / orchestration.** (Reserved for Phase 2, manually triggered.)
* Cloud deployment, CI/CD, infrastructure‑as‑code.
* OCR for scanned images; display a warning instead.

---

## 3. User Stories

| ID | Story                                                                                                     | Priority |
| -- | --------------------------------------------------------------------------------------------------------- | -------- |
| U1 | *As a candidate, I can upload my résumé and instantly see how an ATS parses it so I can fix issues.*      | Must     |
| U2 | *As any user, if parsing fails, I get actionable feedback rather than a generic error.*                   | Must     |

---

## 4. Functional Requirements

| Ref | Requirement                                                                                                                                                                      |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| F1  | Provide `/upload` POST endpoint and web form (Drag‑and‑Drop) that accept a single file ≤ 50 MB in the allowed formats. Return HTTP 413 on size violation, 415 on MIME violation. |                               |
| F2  | Parse the résumé into JSON with the following top‑level keys: `personal_details`, `work_experience`, `education`, `skills`, `certifications`, `summary`.                         |
| F3  | Detect and list parsing issues, e.g. *"Start date missing for Job #2"*.                                                                                                          |
| F4  | Surface results via JSON API **and** a minimal HTML page using server‑side templates (Jinja2).                                                                                   |
| F5  | Log errors and access events; do **not** log raw résumés.                                                                                                                        |

---

## 5. Non‑Functional Requirements

* **Performance**: Parse ≤ 3 s for a 10‑page PDF on a 4‑vCPU dev laptop.
* **Security**: Sanitize all filenames, enforce MIME whitelist, scan uploaded binaries with ClamAV (optional), set “Content‑Security‑Policy” and disable inline JS.
* **Reliability**: 99 % success rate on parsing well‑formed PDFs/DOCs.
* **Accessibility**: WCAG 2.1 AA for the web form.
* **Maintainability**: PEP 8, full type hints, `ruff` + `black` clean.

---

## 6. Technical Guidance (for Copilot)

| Category              | Guidance                                                                          |
| --------------------- | --------------------------------------------------------------------------------- |
| **Language**          | Python 3.12                                                                       |
| **Web Framework**     | `FastAPI` (async, typed endpoints, auto‑docs)                                     |
| **Parsing Libraries** | `pdfminer.six`, `python-docx`, `spacy` for NER, fallback to `textract`.           |
| **Tests**             | `pytest`, `httpx` test client, `pytest-cov` ≥ 90 %                                |
| **Tooling**           | `poetry` for dependency management; pre‑commit hooks (`ruff`, `black`, `pytest`). |
| **Docs**              | `README.md` (setup, run, architecture); inline docstrings (Google style).         |
| **CI Hint**           | *Add GitHub Actions skeleton but do **not** add Docker build steps.*              |

---

## 7. Acceptance Criteria

1. End‑to‑end flow works locally (`uvicorn main:app --reload`).
2. Swagger UI (`/docs`) allows upload and returns expected JSON.
3. HTML form renders parse summary, issues, and diff clearly.
4. Invalid uploads are rejected with correct HTTP codes and messages.
5. **No** `Dockerfile`, `docker‑compose`, or Kubernetes artefacts present.

---

## 8. Milestones & Timeline

| # | Milestone           | Deliverable                         | Target Day |
| - | ------------------- | ----------------------------------- | ---------- |
| 1 | Project Scaffolding | Repo, Poetry, CI skeleton           | +2 days    |
| 2 | Upload Endpoint     | File validation, storage, tests     | +4 days    |
| 3 | Parsing Pipeline    | JSON output, issue detection        | +7 days    |
| 4 | UI & Polish         | HTML templates, README, screenshots | +11 days   |

*Days are relative to kick‑off.*

---

## 9. Risks & Mitigations

* **Complex formatting**: Use robust parsers; surface unparsed sections as issues.
* **Large DOC files**: Stream parse rather than load entire document.
* **Scanned images (no OCR)**: Display warning to user; mark as unparsed.

---

## 10. Glossary

* **ATS** – Applicant Tracking System: software that extracts structured résumé data for recruiters.  

---

## 11. Phase 2 Placeholder (Not in Scope)

When manually triggered, Phase 2 will containerise the app (Docker), add CI/CD pipelines, and prepare deployment manifests. **No Phase 2 work is to be included in Phase 1 deliveries.**
