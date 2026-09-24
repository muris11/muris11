# Premium Profile README Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Redesign `README.md` into a polished premium editorial cyber-tech GitHub profile and remove workflow duplication without adding dependencies.

**Architecture:** Keep repository as documentation-only. `README.md` becomes the single presentation layer, using a restrained set of external SVG/stat providers with readable fallback text. Each GitHub Action owns one output concern: blog, metrics, or contribution snake; no two workflows update the same asset or README section.

**Tech Stack:** GitHub-flavored Markdown, remote SVG/image endpoints, GitHub Actions YAML, PowerShell/Bash link and syntax checks.

**Spec:** User-approved Premium Editorial Cyber-Tech redesign in conversation; research sources listed in final report.

## Global Constraints

- Preserve truthful project, education, experience, contact, and technology data from current `README.md`.
- Use dark-tech foundation with editorial spacing and restrained neon accents.
- Keep animation functional: one hero typing animation, one contribution animation, dynamic data only where useful.
- Keep README scannable; prefer 6–8 featured projects over exhaustive repetition.
- Every image must have useful `alt` text.
- Do not add npm, Python, or other dependencies.
- Keep snake generation in one workflow only.
- Pin or use stable action versions; never use `@master` for credentialed workflows.
- Validate YAML, Markdown structure, links, and changed-file diff before claiming completion.

---

### Task 1: Replace README with premium editorial structure

**Files:**
- Modify: `README.md`

**Interfaces:**
- Produces the complete profile presentation consumed by GitHub profile rendering and dynamic README actions.

- [ ] **Step 1: Preserve verified source facts before editing**

Extract current factual values from `README.md`: identity, portfolio, social links, education, GPA, experience periods, project URLs, and stack. Keep only claims already present unless a public source verifies a correction.

- [ ] **Step 2: Write hero and positioning sections**

Use a centered hero with one capsule header, one typing SVG, concise role line, and four links. Keep animated lines limited to:

```text
Building useful web, mobile, and AI products;Full-stack systems with clean user experiences;Open to meaningful collaboration
```

Add readable text below images so the profile remains understandable when external images fail.

- [ ] **Step 3: Write concise professional profile sections**

Create `About`, `What I Build`, and `Technical Stack` sections. Group stack into Languages, Frontend, Backend, Mobile, AI/Data, Databases, and DevOps. Remove unsupported or duplicate technology claims where current README does not support them.

- [ ] **Step 4: Rewrite featured projects into consistent cards**

Keep the strongest 8 current projects across AI, Business, Education, and Mobile. For each use identical fields: one-line outcome, stack, capabilities, and Live/Source links. Use Markdown headings and tables only where they improve scanability; avoid deeply nested HTML tables.

- [ ] **Step 5: Add experience, education, certifications, and writing**

Use compact tables with consistent date formatting. Keep experience impact-oriented. Add a small blog section with the existing Dev.to marker expected by `blog-post-workflow.yml`.

- [ ] **Step 6: Add restrained analytics and contribution proof**

Keep GitHub stats, top languages, one profile-details card, and snake. Remove redundant streak/productive-time/repo-language cards and remove the Cyclic activity graph endpoint. Each image gets descriptive `alt` text.

- [ ] **Step 7: Add accessible footer and fallback text**

Finish with collaboration CTA, contact links, and plain-text profile URL. Avoid unverifiable quote attribution. Keep visual footer optional and low-height.

- [ ] **Step 8: Inspect Markdown diff**

Run:

```bash
git diff --check -- README.md
git diff --stat -- README.md
git diff -- README.md
```

Expected: no whitespace errors; README contains one heading for each major section and no duplicate analytics/project sections.

---

### Task 2: Make GitHub workflows non-overlapping and safer

**Files:**
- Modify: `.github/workflows/snake.yml`
- Modify: `.github/workflows/update-readme.yml`
- Modify: `.github/workflows/metrics.yml`
- Modify: `.github/workflows/blog-post-workflow.yml`

**Interfaces:**
- `snake.yml` owns generated assets on `output` branch.
- `update-readme.yml` must not generate snake or write the same README regions as other workflows.
- `metrics.yml` owns metrics output.
- `blog-post-workflow.yml` owns the blog marker section.

- [ ] **Step 1: Remove duplicate snake generation**

Delete the `Generate Snake Animation` and related `dist` output from `.github/workflows/update-readme.yml`. If no WakaTime update target is configured by that action, remove the whole redundant workflow instead of retaining a workflow that only creates uncommitted files.

- [ ] **Step 2: Add least-privilege permissions**

Set workflow-level permissions to read by default. Give only the snake publishing job the write permission needed by its publishing action. Do not expose write permissions to metrics or blog jobs unless their documented behavior requires it.

- [ ] **Step 3: Replace mutable action refs**

Change `anmol098/waka-readme-stats@master` to a stable release tag or verified full commit SHA. Change `lowlighter/metrics@latest` to a stable release tag or verified full commit SHA. Keep `Platane/snk@v3`, `actions/checkout@v3`, and other tags only if the chosen verification confirms them; otherwise pin to verified SHAs.

- [ ] **Step 4: Prevent write collisions**

Add workflow `concurrency` groups where workflows can overlap. Keep schedules separated by responsibility. Do not configure multiple workflows to auto-commit the same README marker or generated file.

- [ ] **Step 5: Validate workflow structure**

Run a YAML parser available on the machine, or use a small Python standard-library check if no parser is installed. Confirm every workflow has a trigger, job, runner, and valid `uses`/`with` indentation.

---

### Task 3: Run link, image, and content verification

**Files:**
- Test: temporary command output only; do not commit generated test files.

**Interfaces:**
- Consumes final `README.md` and all workflow URLs.
- Produces verification evidence for completion report.

- [ ] **Step 1: Extract and check URLs**

Use a script or shell command to extract `http`/`https` URLs from `README.md`, then request each URL with a short timeout. Separate expected dynamic image endpoints from portfolio, social, source, and live URLs. Record failures without rewriting valid links blindly.

- [ ] **Step 2: Check image accessibility**

Verify every Markdown image and HTML `<img src>` endpoint returns an image content type or a known redirect. Confirm every image has `alt` text.

- [ ] **Step 3: Check README markers**

Confirm the blog workflow marker exists exactly once and the snake URL points to the `output` branch. Confirm no stale Cyclic activity graph URL remains.

- [ ] **Step 4: Check claims and consistency**

Search for conflicting degree names, duplicate project names, duplicate section headings, unsupported metrics, and inconsistent date formats. Fix only factual or presentation defects found in the final diff.

---

### Task 4: Final verification and integration decision

**Files:**
- Inspect: `README.md`
- Inspect: `.github/workflows/*.yml`

- [ ] **Step 1: Run repository checks**

Run:

```bash
git diff --check
git status --short
```

Expected: no whitespace errors; only intended README/workflow files and optional plan documentation are changed.

- [ ] **Step 2: Review rendered presentation**

Open the GitHub Markdown preview or render the README in a browser-sized viewport. Check desktop and narrow width for broken tables, clipped images, excessive repeated animation, and unreadable contrast.

- [ ] **Step 3: Review security-sensitive workflow changes**

Confirm no secret values entered files, permissions are minimal, action refs are stable, and no workflow can recursively trigger uncontrolled commits.

- [ ] **Step 4: Report limitations honestly**

List any external endpoint returning non-200, provider requiring GitHub execution to validate, or claim that could not be independently verified. Do not call the work complete until all local checks pass or failures are explicitly reported.
