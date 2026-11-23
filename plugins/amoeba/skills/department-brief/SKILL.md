---
name: department-brief
description: Show a department's Executive Brief from Amoeba AI - the verdict, the measures that moved, the decisions to approve and the checklist behind them. Use when the user asks for a department's brief, executive brief or CMO brief, asks what to approve or delegate this week, or wants the brief as text for Slack or as a checklist.
---

# Department Executive Brief

Amoeba writes an Executive Brief for a department: a one-page report an executive reads in about a minute. This skill hands over the latest one. Amoeba writes the brief; here it is only read and presented.

## Getting it

1. `list_departments` for the department's `id`, unless you already have it.
2. `get_department_brief` with that `id`.

The tool answers in one of three ways:

- **A `brief`.** Present it as described below.
- **`brief: null` with a `hint`.** The department has no brief, or none this user may view. Say so in your own words, including where the hint says to create one. Do not assemble a brief from other tools. You may offer, in one line, the department's current state instead; if the user takes it, answer with the `amoeba` skill and say that it is the current state, not a brief.
- **A "not found" error.** The id is not a department of this organization. List the departments again.

If `get_department_brief` is not among the connector's tools, the connector was connected before the tool existed. Ask the user to disconnect and reconnect the Amoeba connector.

## What comes back

`department.name` is the department. `brief.url` opens the report in Amoeba, where the PDF is. `brief.generatedAt` is when it was written. `brief.data` is the brief:

| Field | What it holds |
| --- | --- |
| `verdict` | Where the department is winning and where it is losing. |
| `measures[]` | The numbers that moved: `label`, `from`, `to`, `period`, the objective it belongs to (`space`) and `good`, true when the move is good for the department. |
| `decisions[]` | Up to three approvals: `title`, `delegateTo`, `due`, `actions`, `impact` and the objectives they rest on (`spaces`). |
| `actions[]` | The checklist, one group per Function: `function` and its `items[]`, each with `do`, `detail`, `owner`, sometimes a `space`, and `star`, true when the item carries out one of the decisions. |
| `functions[]` | The map: each Function has `teams[]`, each team has `spaces[]`. A space is an objective: its `name`, its `trend`, sometimes a one-line `objective`, and `points` with what is `changing`, why it `matters` and how to `act`. |
| `asOf` | The last day the brief covers. |
| `counts` | How many objectives are progressing, stable and slipping. Objectives that were not assessed are not counted. |
| `previous` | Only when this report has an earlier run: that run's `asOf` and `counts`. |

Read these as follows:

- "Slipping" is the reader's word for `DECLINING`.
- A null `trend` means the objective was not assessed, and its `points` are empty.
- A Function can have no teams and a team no objectives. Show them as such.
- `brief.window` can come back with null dates. Use `asOf` for the date.
- `period`, `detail`, `owner`, the single `space` of a measure or an item, and an objective's `objective` or any of its `points` can be missing or null. Leave that part of a line out, with its parentheses and its separator.
- With no `previous`, say nothing about earlier briefs. It does not mean this is the first one.

## Presenting it

Start with the 60-second view, under these labels and in this order:

1. The verdict, as written, with no label.
2. **What moved.** One line per measure: `label: from → to (period)`, then `good` or `bad`. With no measures, write that none moved in this period.
3. **To approve.** One line per decision: `title (delegateTo, by due)`.
4. A closing line with the `asOf` date, the counts and the link to the report. With a `previous`, add each count that changed since `previous.asOf`, as `slipping 2 → 3`.

Then say in one line what else the brief holds, and show it only on request:

- **Behind an approval:** its actions, its impact and the objectives it rests on.
- **The checklist:** the items by Function. A ★ marks the ones that carry out an approval.
- **The map:** the objectives of a Function or a team, each with its trend and its points.

Keep to these rules:

- Use the brief's own wording, numbers and order. Do not rewrite it, re-rank it or add findings from other tools into it. If you add something from elsewhere, say that it is not from the brief.
- Answer in the user's language. Names and numbers stay exactly as they are.
- Write `asOf` and `due` as a short date, such as "Nov 26". Leave `period` as it comes.
- The brief is a snapshot as of `asOf`. A space can show a different trend today. When the user asks about now, check with `get_space` or `get_department` and give both dates.
- An existing brief is regenerated in Amoeba from its report page, at `brief.url`. A first one is created from the department's Reports tab. Neither can be done from here.

## As text for Slack

When the user wants to paste the brief into Slack, give this in a code block. Leave out "What moved" when there are no measures, and the "Since" line when there is no `previous` or no count changed.

```
*<department.name> Executive Brief* (as of <asOf>)
<verdict>

*What moved*
• <label>: <from> → <to> (<period>), <good or bad>

*To approve*
• <title> (<delegateTo>, by <due>)

<progressing> progressing, <stable> stable, <slipping> slipping
Since <previous.asOf>: <count> <before> → <after>
<brief.url>
```

## As a checklist

When the user wants the list of changes, give this in a code block, one group per Function. An item with no `owner` is the `do` alone:

```
<function>
- [ ] ★ <do> (<owner>)
- [ ] <do> (<owner>)
```
