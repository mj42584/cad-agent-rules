# Working rules for the agent — 3D design project

*Version 0.1*

Generic rules, valid for any 3D design project: copy this file as is from one project to the next. Everything specific to a project (software versions, version control tool, connector working folder, linked files, pitfalls) lives in the **design file** ([DESIGN.md](DESIGN.md), chapter "Project tools and pitfalls"); its chapters keep the same names from one project to the next (§3).

## 1. Principles

- **Free, no need to ask**: read the project files, inspect the model (parameters, bodies, errors, measurements, interferences), look at the version history.
- **Explicit approval required**: any change to the 3D model, any save of the model, copying a file into the project, saving a version, installing software, running calculations or simulations. Calculations are **never re-run automatically**, even after a parameter change.
- **Approved plan**: the agent announces a numbered plan; the user's "yes" covers the whole plan. At the slightest deviation (unexpected issue, error, unplanned step), the agent stops and asks again.
- **No made-up data**: any data not provided (dimension, material, load, steel grade…) is asked of the user, or used with the mention "assumed" in the design file, never presented as established.
- **Status of calculations**: the agent's calculations are orders of magnitude; never present them as a validated structural calculation, which remains the job of a qualified engineering firm.
- **Concise answers**: the result first, then only what needs a decision from the user. Short sentences and lists, no repeating the design file, no options that are not retained; details go in the file, not in the answer.

## 2. Course of a task

**Version control**: these rules work with Git, a PLM or a plain file manager. They use four notions; the design file says which tool the project uses and how each notion is done in it.

| Notion | Git | PLM | File manager |
|---|---|---|---|
| **Working version**: changes without touching the reference | branch | check-out | copy in a working folder |
| **Save a version** | commit + push | new revision (check-in) | dated or indexed copy |
| **Reference version** | `main` | released revision | "approved" folder |
| **Approve**: the user reviews, then integrates | merge | approval workflow | copy to the "approved" folder by the user |

1. **Observe**: read the software versions and compare them with those in the design file (§4); read the state of the model (driving parameters, number of bodies, errors) and compare it with the design file, to spot changes made by hand between two sessions. Nothing is written yet.
2. **Propose** the plan, with the differences observed, and wait for the "yes".
   Then, **before any change**, create a **working version** from the up-to-date reference version (short task name). Never change the reference version directly: the user approves, after reviewing the changes to the design file. Only one working version at a time on a binary file (3D model): two versions changed in parallel cannot be merged.
   First write in the working version: the differences observed, in the history.
3. **Carry out**, then **test**: vary one or two relevant parameters and set them back, 0 errors. If a part of an assembly has changed: update the links, then check interferences.
4. **Exports** (list in the design file): if the **3D geometry** has changed, regenerate them. Not for a change with no effect on geometry (renaming, material, reorganising the tree): note it in the history.
5. **Document**: update the design file (and the README if what it describes changes), add a line to the history.
6. **Save** (connector working folder → project → save a version), or write explicitly "not saved" in the history. Then **propose approval**: a message to the user summarising what the working version contains (or the request the tool provides: pull request, approval workflow…). If the agent has no access to the version control tool, it prepares the files and the user saves.

## 3. Keeping the design file

**File outline** (`DESIGN.md`):

| Chapter | Content |
|---|---|
| 1. Goal | What is being designed and what must be parametric |
| 2. Input data | Where the data comes from (site measurements, starting values, catalogues); current values are in chapter 3 |
| 3. Model parameters | Driving (value, what they drive) and computed (expression, value): the only place where values are written |
| 4. Model architecture | Coordinate system, construction tree, environment and assembly |
| 5. Decision log | D1, D2…: the decision and its reason |
| 6. Project tools and pitfalls | Software versions, files and folders, known pitfalls, software review, version control |
| 7. Open points | At the top, "Calculations to re-run" when needed; then only unresolved questions |
| 8. Next steps | List set by the user, and what is on hold |
| 9. History | Table date / request / done / files |

- **One piece of information, one place**: a parameter value is written only in the parameter table; elsewhere, the parameter is referred to by name. A derived dimension is copied only when useful, with the configuration it applies to. A pitfall is described only in the pitfall list; elsewhere, refer to it.
- Only the history keeps dated values; it is never rewritten.
- **Outdated calculations**: as soon as a parameter used by a calculation changes, add at the top of the open points "Calculations to re-run: <changed parameters> (calculated with <configuration>)". The line is removed when the user re-runs the calculations.
- Since the 3D model is binary, **the design file is the readable description of changes**: every change to the model must be described in it. Without a version control tool, it is the only record.

## 4. Software and versions

- The design file holds the **software version table**: software, role, version, command to check it.
- **Review at every update**: for each software whose version has changed, read the intermediate release notes, cross-check them with the project's workarounds and pitfalls, note what may change next to the software version table, then update that table. Test only with approval.
- A new version of the CAD software may change the file format; a new version of the calculation libraries may change the results: then add the line "Calculations to re-run".
- **Linked files** (assemblies): keep them together, links as relative paths.
- **Session without access to the CAD software** (agent in the cloud, another computer): it has its own software versions, no review is done there; it is used for documentation, calculations and audits. 3D work is done on the computer that has the software; the version control tool links the two.

## 5. Pitfalls

The design file holds the list of project pitfalls, one line per pitfall: the symptom, the workaround, and the version from which it may be fixed.

## 6. Modelling

- **A model a human can edit**: always prefer the method a user can take over by hand in the CAD software, even if another one is faster for the agent. For example: in sketches, **dimensions** driven by parameters rather than points computed by expressions; named parameters rather than hard-coded values.
- **Rename everything**: every element created (sketch, feature, body, plane, parameter, component) immediately gets an explicit name saying what it is and where it is (e.g. `Stringer profile F1`, `Outer stringer F2`), never the software's default name (`Sketch3`, `Extrude12`). A human must be able to find their way in the tree without reading the design file.
- **Build from a skeleton**: geometry relies on master sketches and reference planes driven by parameters, never on faces or edges of other bodies, which change or disappear when a parameter changes.
- **Fully constrained sketches** (0 degrees of freedom): otherwise they may move without warning on rebuild.
- **One body per real part**: every manufactured or purchased part is a separate body; this is what makes the bill of materials, materials and interference checks possible.
- **Pattern rather than copy**: a repeated element is produced by a pattern whose count and spacing are driven by parameters, not by independent copies.
- **Mirror rather than redraw**: a part symmetric to another is produced by a mirror about a reference plane, not redrawn; preferably a mirror of the whole body, more robust than mirroring features (which may merge with the original body).
- **Material at creation**: assign the material as soon as a body is created.
- **Declared simplifications**: model only what is needed (no detailed fasteners or fillets unless asked); every simplification is noted in the design file with its effect (mass, cross-section…).
- When the software forces a less readable method, record it as a workaround in the project pitfalls.

## 7. Project tree

| Path | Content |
|---|---|
| `README.md` | Showcase: what the project is, picture, 3D view, how it is made |
| `AGENTS.md` | Entry point for an agent: reading order |
| `AGENT_RULES_3D.md` | These rules |
| `DESIGN.md` | Design file: project state, decisions, tools, pitfalls, history |
| `cad/` | 3D models (parts, environment, assembly) and their exports (STL, previews, drawings) |
| `calc/` | Calculation scripts; `calc/report/`: calculation report and figures |
| `archive/` | Old versions and original files, never modified |

A new kind of file goes in the existing folder that matches it; create a folder only if none fits, and add it to this table. If the version control tool (a PLM) imposes its own structure, the design file says where each element is.
