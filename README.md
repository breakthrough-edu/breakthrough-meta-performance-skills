# meta-performance-skills

Two Claude Code skills for a student running Meta ads for their own business.

| Skill | Job |
|---|---|
| `performance-db-setup` | One time. Installs the Lark Base template on the student's own tenant, wires the Meta sync, and files a pointer note in their vault. |
| `performance-analysis` | Weekly. Reads that Base, refuses to advise when the data cannot support advice, and classifies what is actually wrong. |

**Two skills, not one, for a boring reason:** the weekly skill would otherwise carry the entire install manual in its context every single week, for a job that happens once in the life of the account.

## Where they run

Both run **inside a vault built by `my-second-brain`**, and **ship separately from it**. They are installed and updated on their own. Each session reads that vault's own `99_Meta/structure-doctrine.md` **live**, and neither package carries a copy of it: a second copy of someone's constitution drifts the first time either one is edited.

If a student has no vault, the setup stops and sends them to install one. It does not degrade to writing a plain note. A note in a shape the vault does not recognise is worse than no note, because it looks like a record and cannot be found by anything that reads records.

## Install

```
npx skills add -g breakthrough-edu/meta-performance-skills
```

One command installs both. The `-g` installs them for your user; without it they land in whatever folder you happened to be standing in.

Each skill's `SKILL.md` sits inside its own `<skill-name>/` subdirectory, which is what lets `npx skills add` find both of them, and why the repo root holds two folders and this file rather than a `SKILL.md`.

**Both are manual-trigger only.** Neither fires on its own. The student asks for it by name.

## The skills promise nothing

⛔ There is no install-time claim anywhere in this repo, and none is to be added. Nothing says "installs in one session", nothing estimates how long a gate takes. Parts of the setup depend on Meta's review posture and on the state of the student's own ad account, and none of that is ours to promise. When a gate cannot be passed, the session names which one and stops.

## File map

```
meta-performance-skills/
├── README.md                                  this file
├── performance-db-setup/
│   ├── SKILL.md                               the setup's gates, in order
│   └── references/
│       ├── changing-the-base.md               the law, copy 1 of 2
│       ├── vault-note.md                      the note the setup writes
│       ├── meta-token-and-sync.md             token provenance, the sync wiring, runtime checks
│       └── ...                                further on-demand references
└── performance-analysis/
    ├── SKILL.md                               the opening checks and the classification
    └── references/
        ├── changing-the-base.md               the law, copy 2 of 2
        ├── verdict-snapshot.md                the snapshot the analysis appends
        └── ...                                further on-demand references
```

## Maintenance covenant 1: one law, two homes

`performance-db-setup/references/changing-the-base.md` and `performance-analysis/references/changing-the-base.md` are **the same law shipped twice**, because each skill has to carry it into context alone.

**They are byte-identical.** `diff` the two files; anything it returns is a bug:

```
diff performance-db-setup/references/changing-the-base.md \
     performance-analysis/references/changing-the-base.md
```

⛔ **Every future edit lands in both files in the same change.** Never one now and the other later. "Later" is how two documents that both look authoritative start handing contradictory instructions to two sessions working on the same Base, and neither session has any way to tell which copy is the stale one.

## Maintenance covenant 2: the vault contract has two halves

The vault note the setup writes and the dated verdict snapshot the analysis appends to that same note are **one contract described in two files**: `performance-db-setup/references/vault-note.md` and `performance-analysis/references/verdict-snapshot.md`.

**Edit them with each other in view.** If the snapshot's shape drifts from the note's, the analysis appends to a note whose structure it no longer matches, and week 2 loses week 1's advice. That loss is silent: the note still exists and still looks fine.

## Why the skills inspect instead of assume

**Both skills are continuously iterated. The template Base is not, and cannot be.**

A student's Base is a copy taken on the day they installed. `+base-copy` carries the tables, the records, the field descriptions, the formulas, the dashboards and the workflows, and the copy binds to itself rather than back to the original, which is precisely what makes it **a snapshot and not a subscription**. Any fix made to the template after a student has copied it will never reach that student.

So:

- The skills **detect the Base's shape at run time** rather than assuming a version.
- ⛔ There is **no version-stamp system**, deliberately. A stamp is a claim about the Base, and it goes stale the moment someone edits by hand in the UI. Inspection cannot go stale.
- Every legal change to a student's Base is written so that a later session can detect it as already-applied.

The rules for that, and the measured Lark behaviour they are built on, are in `references/changing-the-base.md`.
