# Agent rules for 3D design projects


Working rules for designing a 3D model with an AI agent (Claude Code or similar) driving CAD software: who decides, how the model is built, how changes are recorded and documented.

| Language | File |
|---|---|
| English | [AGENT_RULES_3D.md](AGENT_RULES_3D.md) |
| Français | [REGLES_AGENT_3D.md](REGLES_AGENT_3D.md) |

**SolidCraft + GitHub + Claude Code** (French, shorter, tool-specific): [REGLES_SOLIDCRAFT_GITHUB_CLAUDE.md](REGLES_SOLIDCRAFT_GITHUB_CLAUDE.md).

## What they are for

An agent that drives CAD software works fast, but leaves a model that is hard to follow: changes nobody approved, unnamed features, values hidden in expressions, decisions that only exist in a past conversation. These rules keep the human in charge and the project readable:

- the agent proposes, the user approves, and nothing changes without approval;
- the model stays editable by hand: dimensions in sketches, named parameters, explicit names, built from a skeleton;
- every change is described in a **design file**, which is the readable record of a binary 3D model;
- it works with Git, a PLM or a plain file manager.

## How to adopt them

1. **Copy** the rules file of your language into the project root, unchanged.
2. **Create `AGENTS.md`** with one line: *"Before any action, read [AGENT_RULES_3D.md](AGENT_RULES_3D.md) in full (working rules), then [DESIGN.md](DESIGN.md) (project state, decisions, tools and pitfalls)."* Claude Code reads `CLAUDE.md`: if your agent does not read `AGENTS.md`, add a `CLAUDE.md` that points to it.
3. **Create `DESIGN.md`** with the nine chapters listed in §3 of the rules (Goal, Input data, Model parameters, Model architecture, Decision log, Project tools and pitfalls, Open points, Next steps, History).
4. **Fill in with the agent**, before modelling: the goal, the input data, the software version table and the version control tool (chapter 6).

## Limits

- **Tested on a single project** ([a parametric staircase](https://github.com/mj42584/staircase_solidcraft_claude), SolidCraft + Claude Code), over a few days. Some rules have not yet been through a real change cycle.
- **Not validated by a study.** The few existing studies on agent rule files disagree on how much they help; they suggest keeping the file short, putting the most important rules first, and writing it yourself rather than letting an AI generate it.
- **Written mostly with an AI**, then directed and reviewed by a human.
- The agent's structural calculations are orders of magnitude, never a validated calculation.

Feedback and pull requests are welcome.

## License

[MIT](LICENSE)
