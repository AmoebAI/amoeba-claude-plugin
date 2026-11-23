---
name: amoeba
description: Read an organization's business health through the Amoeba AI connector. Use when the user asks how a department, team, space, initiative or exploration is doing, wants to find one by name, or asks what Amoeba has analyzed about a business question.
---

# Reading Amoeba

Amoeba tracks how an organization's business is going. Everything below is reachable through the `amoeba` connector; sign in to it first if its tools are not available.

## How things are organized

- The organization has **departments**. Marketing, Revenue and Customer Success are built in; custom ones may exist.
- A department groups **Functions**, a Function groups **Teams**, and a Team owns **spaces**. An "initiative" is a space under a team, not a separate entity.
- A **space** is a workspace around a business question, backed by connected data sources, with reports, explorations and agents.
- An **exploration** is one analysis: the data behind a question and the goals, insights and recommendations it produced. It lives in a space, and the same exploration can appear in more than one.

## Health vs. lifecycle

- Departments, Functions, Teams and spaces carry a **trend** from their latest analysis: `PROGRESSING`, `STABLE` or `DECLINING`, or null when none has run yet.
- A space's `status` field is its **lifecycle** (`READY`, `DRAFT`, `ARCHIVED`), not its health. Skip archived spaces unless the user asks for them.

## Which tool to use

| Need | Tool |
| --- | --- |
| Find anything by name | `search`, then `fetch` to read it |
| Browse the hierarchy | `list_departments`, then `get_department` for its Functions, Teams and spaces with trends |
| Spaces the user can see | `list_spaces` (filter by `query`, page with `cursor`) |
| One space in depth | `get_space` |
| Analyses in a space | `list_explorations` with `spaceId`, then `get_exploration` for goals, insights and recommendations |
| A department's Executive Brief | `get_department_brief`; the `department-brief` skill says how to present it |

Every id comes from a previous result; never guess one. When pointing the user to an entity, share the `url` the tool returns.

## Answering well

- Lead with the trend and what drives it, then the detail. A status count is not an answer on its own.
- Use only the numbers and names the tools return.
- If the connector is not connected or returns a permission error, say so and ask the user to connect their Amoeba account rather than retrying.
