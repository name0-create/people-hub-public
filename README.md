# People Hub — Obsidian Plugin

Personal CRM built around your Carnegie system. Every piece of data lives in your Markdown notes. The plugin reads and writes frontmatter only — no hidden databases.

---

## Install

Only two files are needed in the plugin folder — **the stylesheet is bundled into `main.js`** and injected at load, so the UI is styled even if `styles.css` is missing:

```
<vault>/.obsidian/plugins/people-hub/
├── main.js          ← compiled output (npm run build)
└── manifest.json
```

(`styles.css` is the source of the bundled styles; copying it too is harmless.) Requires Obsidian **1.6.6+**.


## Frontmatter it reads

| Concept | Keys (in priority order) |
|---|---|
| Name | `name`, `display_name`, `full_name`, `title`, file name |
| Type | `type_person` |
| Tier | `prm-tier`, `circle`, `tier` — derived from `type_person` if absent |
| Status | `status` |
| Last contact | `last_contact`, `last_contacted` |
| Next contact | `next_contact`, `next_encounter`, `next_action_date` |
| Frequency | `frequency` |
| Birthday | `birthday`, `birthdate`, `birth_date`, `dob` |
| Anniversary | `anniversary`, `friendiversary`, `wed_date`, `anniversary_date` |
| Anniversary label | `anniversary_label`, `anniversary_type` |
| Health / Trust | `health_score`, `trust_score` |
| Carnegie | `c1_score` … `c9_score` (0–5) |
| Promises | `promises_made`, `promises_kept` |
| Socials | `ig`, `linkedin`, `youtube`, `x_twitter`, `phone`, or nested `socials:` |
| Talk log | `talk1_date/note/learned/next/where/presence/energy` … `talk5_*` |
| Snooze | `snoozed_until` (written by the ⋯ → Snooze menu) |

---

## People directory (main window)

Full-page card grid of everyone in the index: photo or initials, name, company/role, last contact, favourite star. Search (name, company, role, type, tags), sort (name, last contact, next encounter, favourites first), company and tag filters. Click opens the note (Ctrl/Cmd or middle-click → new tab; right-click → log talk / quick log). Updates live as notes change. Archived people are hidden. The `photo` property may be a URL, `[[wikilink]]`, `![[embed]]`, markdown image or vault path.

---

## @ mentions

Type **@** in any note and pick a person. The note stores the stable key (`@JohnDoe`), and it shows as the person's name in
Reading View and Live Preview. Hover a mention for a short card; click it to open the person. Keys are set on each person
(`mention_key`), generated from the name, and kept unique. Changing a person's display name never changes their key.

Commands: **Find References to a Person**, **Generate Missing Mention Keys**, **Validate Mention Keys**,
**Rebuild Mention Index**. Settings → @ Mentions controls the popup, rendering, hover fields, and what happens when a key changes.

---

## Person profile

Click a person on the People page to open their profile. The header has the photo, name, relationship and role,
★ Favourite, Edit, Open Note and Delete. The tabs are Overview, Relationship, Interactions (timeline and **+ Log Interaction**),
Meetings, and Notes (the note's Markdown body). Ctrl/Cmd-click a person to open the note instead.

Log Interaction writes into the person's file under Talks Log, and sets last contact and the next encounter: the follow-up date,
or today plus the cadence.

---

## People Hub sidebar

Open with the panel icon or the command **People Hub: Open sidebar**. The top half is global: **+ Person**, **Today** counts,
**Navigation**, and **Groups**. The bottom half is the **active person** (the open note): photo, relationship, company, last and
next contact, birthday, quick actions (Interaction, Meeting, Contact, Snooze), and open loops and promises.

Today's counts: *Reach-outs* = due or overdue (same as the Due tab); *Birthdays* = within seven days; *Meetings* = planned today.

## Search and filters (People page)

- **Search** matches name, aliases, company, role, relationship type, category, groups, interests, focus, tags,
  location, email and phone. Every word must match. `#area` searches tags, `@nairobi` searches location.
- **Advanced filter** takes conditions such as `company = Acme AND relationship_type contains client AND status = active`.
  Use `OR` and parentheses to combine. Operators: `=`, `!=`, `contains`, `not contains`, `starts with`, `ends with`,
  `>`, `>=`, `<`, `<=`, `in`, `not in`, `is empty`, `is set`. Quote values with spaces inside keywords: `role = "Head of AND Product"`.
  An invalid expression shows why and keeps the last valid filter.
- **Sort** by name, last or next contact, favourites, company, role, status or importance.

---

## Talk log and interactions

An interaction is a talk. Both live in the person's file under **Talks Log**, in one format. The **Interaction** quick action
logs a typed talk there; the **Interactions** list in the sidebar shows every talk across all people.


The talk log lives in the person's note, under **## Talks Log**. In **Reading view** that list is shown as a timeline
(stats, dated cards, presence dots, Carnegie pills, next step). The file keeps plain bullets, so it still reads well in any app.
Live Preview (editing) shows the bullets as text.

Older per-talk documentation:

Each person's **Talks Log** section is shown as a timeline: dated cards, grouped by month, newest first, with a presence meter, learned
notes, Carnegie pills and next steps. The person dialog shows the latest three; **Show all** opens the full log with totals (talks,
days since last, average presence, last 30 days). The timeline is read-only: the note stays the source of truth, and the parser
reads both formats the plugin writes (sidebar quick-log and CRM dialog), plus hand-written lines.

---

## People page — cards and filters

Each person is a card: favourite star (click to toggle, does not open the note), photo or initials, name, status, relationship,
company, last contacted, next contact (overdue / due today / in N days / snoozed), and an upcoming birthday. Choose which fields
show with **Cards**.

The chips **Due · Overdue · Favorites · Birthdays · Paused** filter in place, with live counts — clicking a chip never opens a page.
Chips of the same kind replace each other; company and tag dropdowns and search combine with them. Clear with "Clear filters".

---

## Personal CRM (main window)

Open it from the ribbon (heart-handshake icon, or a fallback icon if your Obsidian lacks it), the command palette ("People Hub: Open Personal CRM"), the **Personal CRM** button on the People page, the **CRM ↗** link in the sidebar, or Settings → People Hub. If you can't find it, check Settings → People Hub shows **v0.3.0** — if not, Obsidian is still running the old `main.js`; fully reload it. Who is due, who is slipping, who isn't classified yet.

- **Header:** *Reach out* (walks the due queue, one person at a time), *Add person*, *Triage* (assign a circle to unclassified people), 📅 (sidebar Birthdays), **Undo / Redo**, refresh. Counts under the title jump to the matching tab.
- **Tabs:** **Due** (overdue + due soon) · **Tracked** · **Follow-ups** · **Drifting** (overdue by a full cadence or more) · **Unclassified** · **Birthdays** · **Paused**.
- **Search:** `mike #area @phone` — words match the name, `#tag` matches by prefix (`#area` finds `#area/health`), `@place` matches the place. Sort: most overdue, name, last contact, circle, most mentioned.
- **Rows:** circle badge (Inner circle / Close / Extended / Professional), status badge (`overdue 7d`, `due in 3d`, `snoozed to …`, `1 follow-up`), last contact, cadence, `N× mentioned` (other notes linking to them), `@place`, `#tags`, and a **progress line** under the row showing how much of the cadence has elapsed (pink overdue · orange due soon · mauve fine). ✓ logs a contact today, ✎ opens the note, ⏰ snoozes. Tick rows for bulk Log contact / Snooze / Pause.
- **Click a row** for the dialog: note excerpt, follow-ups, date + notes (written under the log entry, **line breaks kept**), *Log contact*, *Set cadence*, *Snooze*, *Tags*, *Place*, *Open note*. Click the circle badge to change circle.

**How it decides.** *Tracked* = the person has a circle (`prm_tier`, or a relationship type that maps to one). *Cadence* = the circle's days (Settings → tiers) unless the note has `cadence: 2w` (also `10d`, `3m`, `1y`; words like `weekly` are labels only). The clock starts at the last contact — or, if never contacted, the day tracking began (`first_encounter_date`, else the note's creation date), so a new person isn't overdue on day one. A *snooze* hides someone from Due until its date but keeps their real overdue-ness; logging a contact ends a snooze. "Due soon" is the setting *CRM: due soon window*, never more than half a person's cadence.

**Where it writes** (all through `PersonRepository`, validated and atomic): follow-ups → `open_loops`; place → `place_name`; tags → `tags`; pause → `status: paused`; snooze → `snoozed_until` + `next_encounter`; contact → `last_contacted` + the Talks Log entry. **Undo/Redo** reverses the exact change (frontmatter *and* the log entry), and refuses to overwrite anything you edited since.

---

## Sidebar tabs

### Today
| Section | What it shows |
|---|---|
| 🎂 Birthday today | People whose birthday is today |
| 💍 Anniversary today | People whose anniversary is today |
| 🎂 Upcoming · Nd | Birthdays within the configured look-ahead |
| 💍 Upcoming · Nd | Anniversaries within the configured look-ahead |
| 📞 Reach out | Everyone overdue, sorted by most overdue then tier |
| ⚠️ Carnegie avg < 3 | People needing relationship work |
| 🤝 Promises < 80% | People with low promise-kept ratio |

**Swipe right** on any person in the "Reach out" section to open the quick-log sheet. Or tap the **📝** icon.

### People
Searchable list. Filter by tier or "Due now". Shows type, frequency, last-contact age, health score, Carnegie bar, social links. Swipe-to-log works here too.

### Birthdays
- Stat tiles: **Total · Today · 7d · 30d**
- **Next 30 Days** and **Later Birthdays** cards: name, zodiac + lunar line (*Rooster · Libra · Lunar Sep 18th*, can be switched off in settings), birthday, next date, days away, age they turn
- **Birthday Calendar**: two months, Prev / Today / Next; 🎂 birthdays and 💍 anniversaries as chips, today outlined
- Anniversaries (60 days) and "No birthday / anniversary on file" lists

### Carnegie
- 3×3 grid: average score per principle across all active people
- People below avg 3 — needs work this week
- Full sorted table, worst first — tap **View** for the C1–C9 breakdown modal

---

## Quick-log sheet (mobile)

Opens from swipe-right or the 📝 icon. Optimised for one-handed use:

1. **Type chip strip** — scrollable row of log types (call, whatsapp, coffee, lunch, walk, home-1on1, video, message, email). Large tap targets.
2. **Date** — defaults to today.
3. **Note** — free-text area.
4. **▸ More** — collapses/expands: Where, Learned, Agreed next, Presence /5, Energy /5, Carnegie toggles (C2/C4/C7).
5. **Save** — logs the talk, rotates `talk1`…`talk5` in frontmatter, updates `last_contact`, appends a bullet under `## Talks Log`.

---

## QuickAdd integration

**Pattern A — Execute macro** (recommended):
1. Create a QuickAdd macro named e.g. `New Person` with your Templater template choice inside it.
2. In People Hub settings: enable **QuickAdd**, set **Macro name** to `New Person`.
3. The **+** button and **New person** command will now run your macro.

**Pattern B — User script**:
1. In QuickAdd → User scripts, point to `people-hub-qa-script.js` (copy from the build output).
2. The script calls `plugin.newPerson()` which falls through to the built-in modal.

If QuickAdd fails (plugin not found, wrong macro name), People Hub falls back to the built-in creator silently.

---

## Commands

| Command | Description |
|---|---|
| Open People Hub | Focus/open the sidebar |
| People Hub: Today | Jump to Today tab |
| People Hub: Birthday calendar | Jump to Birthdays tab |
| People Hub: Carnegie scores | Jump to Carnegie tab |
| People Hub: New person | QuickAdd → Templater → built-in modal |
| People Hub: New person via QuickAdd | Macro directly, then built-in |
| People Hub: Log talk | Full log modal (active note or picker) |
| People Hub: Quick-log (one-tap sheet) | Bottom sheet (active note or picker) |
| People Hub: Carnegie for active note | C1–C9 modal |
| People Hub: Insert reach-out list | Paste bullets into editor |
| People Hub: Insert upcoming birthdays | Paste bullets into editor |
| People Hub: Insert upcoming anniversaries | Paste bullets into editor |
| People Hub: Refresh index | Force re-scan |

---

## `people` code block

````
```people
view: reachout
```

```people
view: birthdays
days: 14
```

```people
view: anniversaries
days: 7
```

```people
view: carnegie
```
````

---

## Status bar

`🎂2 💍1 📞5 ⚠️C1` — birthdays today · anniversaries today · reach-out count · low Carnegie count. Click to open the Today tab.

---
