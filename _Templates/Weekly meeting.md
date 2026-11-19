<%*
// Weekly meeting. Scans the WHOLE VAULT for tasks completed in the last 7 days,
// then works out which of last week's actions are still open. Written once, at
// creation -- minutes are a record, not a live query.
const FOLDER = "02 MEETINGS/Weekly";
const DAYS   = 7;
const today  = tp.date.now("YYYY.MM.DD");
const title  = `${today} Weekly Meeting`;
const cutoff = window.moment().subtract(DAYS, "days").startOf("day");

const strip = s => s
  .replace(/[✅➕⏳🛫📅❌]\s*\d{4}-\d{2}-\d{2}/g, "")
  .replace(/[🔺⏫🔼🔽⏬🆔⛔🔁]/g, "")
  .replace(/\[\[([^\]|]+)\|?[^\]]*\]\]/g, "$1")
  .replace(/\s+/g, " ").trim();
const key = s => strip(s).toLowerCase().replace(/[^a-z0-9 ]/g, "").trim();

// ── 1. everything completed vault-wide in the window ───────────────────
const done = [];
for (const f of app.vault.getMarkdownFiles()) {
  if (f.path.startsWith("_Archive") || f.path.startsWith("_Templates")) continue;
  let text;
  try { text = await app.vault.cachedRead(f); } catch (e) { continue; }
  if (!text.includes("✅")) continue;
  for (const line of text.split("\n")) {
    const m = line.match(/^\s*-\s*(?:\[[xX]\]\s*)?(.*✅\s*\d{4}-\d{2}-\d{2}.*)$/);
    if (!m) continue;
    const d = m[1].match(/✅\s*(\d{4}-\d{2}-\d{2})/);
    if (!d || window.moment(d[1], "YYYY-MM-DD").isBefore(cutoff)) continue;
    const txt = strip(m[1]);
    if (txt) done.push({ txt, date: d[1], src: f.basename });
  }
}
done.sort((a, b) => a.date.localeCompare(b.date));
const doneKeys = done.map(d => key(d.txt));

// ── 1b. the Notes section of each daily note in the window ─────────────
const DAILY = "00 DAILY NOTES";
const diary = [];
for (const f of app.vault.getMarkdownFiles()
        .filter(f => f.path.startsWith(DAILY + "/") && /^\d{4}\.\d{2}\.\d{2}$/.test(f.basename))
        .sort((a, b) => a.basename.localeCompare(b.basename))) {
  const d = window.moment(f.basename, "YYYY.MM.DD");
  if (!d.isValid() || d.isBefore(cutoff) || d.isAfter(window.moment())) continue;
  let text;
  try { text = await app.vault.cachedRead(f); } catch (e) { continue; }
  const lines = text.split("\n");
  const s = lines.findIndex(l => /^##\s+Notes\s*$/i.test(l.trim()));
  if (s === -1) continue;
  const body = [];
  for (let i = s + 1; i < lines.length; i++) {
    if (/^##\s/.test(lines[i])) break;
    body.push(lines[i]);
  }
  const txt = body.join("\n").trim();
  if (txt) diary.push({ day: d.format("ddd D MMM"), txt });
}

// ── 2. last week's actions, split by whether they got done ─────────────
const prev = app.vault.getMarkdownFiles()
  .filter(f => f.path.startsWith(FOLDER + "/") && /^\d{4}\.\d{2}\.\d{2}/.test(f.basename)
               && f.basename.slice(0, 10) < today)
  .sort((a, b) => b.basename.localeCompare(a.basename))[0];

const open = [];
if (prev) {
  const lines = (await app.vault.cachedRead(prev)).split("\n");
  const start = lines.findIndex(l => /^\*\*Actions:?\*\*/i.test(l.trim()));
  if (start !== -1) {
    for (let i = start + 1; i < lines.length; i++) {
      const l = lines[i].trim();
      if (/^(#{1,6}\s|\*\*[A-Z])/.test(l) && !l.startsWith("- ")) break;
      const m = lines[i].match(/^\s*-\s*(?:\[(.)\]\s*)?(.+)$/);
      if (!m) continue;
      const txt = strip(m[2]);
      if (!txt || /^\d{4}-\d{2}-\d{2}$/.test(txt)) continue;
      const box    = (m[1] ?? " ").toLowerCase();
      const closed = box === "x" || /✅/.test(m[2]) ||
                     doneKeys.some(k => k && (k === key(txt) || k.includes(key(txt)) || key(txt).includes(k)));
      if (!closed) open.push(txt);
    }
  }
}

const diaryBlock = diary.length
  ? diary.map(d => `**${d.day}**\n${d.txt}`).join("\n\n")
  : "_No daily notes written this week._";
const doneBlock = done.length
  ? done.map(d => `- ${d.txt} -- done ${d.date}`).join("\n")
  : `_Nothing marked complete since ${cutoff.format("YYYY-MM-DD")}._`;
const openBlock = open.length
  ? open.map(t => `- ${t} -- carried over`).join("\n")
  : (prev ? "_Nothing outstanding._" : "_No previous weekly meeting found._");

await tp.file.rename(title);
await tp.file.move(`${FOLDER}/${title}`);
-%>
---
type: meeting
created: <% tp.date.now("YYYY-MM-DD") %>
date: <% tp.date.now("YYYY-MM-DD") %>
people: []
org: COMPACT
---
### From the daily notes
<% diaryBlock %>

### Done this week
<% doneBlock %>

### Still open
<% openBlock %>

## Updates

**Observations:**

**Processing:**

**Issues:**

**Discussions:**

**Actions:**
- 
