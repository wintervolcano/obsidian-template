<%*
// Journal club. Links the Zotero literature note instead of copying its
// title/authors/year, so nothing can go stale or read "Unknown Authors".
const FOLDER = "02 MEETINGS/Journal Club";
const raw = await tp.system.prompt("Citation key (without @):");
if (!raw) return;
const key = raw.replace(/^@/, "").trim();

const lit = tp.file.find_tfile(key);
let shown = key;
if (lit) {
  const fm = app.metadataCache.getFileCache(lit)?.frontmatter;
  if (fm?.title) shown = fm.title;
} else {
  new Notice(`No literature note "${key}" yet -- the link will resolve once Zotero creates it.`);
}

const presenter = await tp.system.prompt("Presenter:", "");
const title = `${tp.date.now("YYYY.MM.DD")} ${presenter || key}`;
await tp.file.rename(title);
await tp.file.move(`${FOLDER}/${title}`);
-%>
---
type: journal-club
created: <% tp.date.now("YYYY-MM-DD") %>
date: <% tp.date.now("YYYY-MM-DD") %>
presenter: <% presenter %>
paper: "[[<% key %>]]"
---
# <% shown %>

#### Summary

#### Discussion

#### Takeaways

> [!question] 
> What is the meaning of this? 
