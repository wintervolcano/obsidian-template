<%*
// The picker stashes the chosen project; fall back to this note's own folder
// in case it was created some other way.
let project = window._tpProject || "";
window._tpProject = null;
if (!project) {
  const m = (tp.file.folder(true) || "").match(/^01 PROJECTS\/([^\/]+)/);
  if (m) project = m[1];
}
const projectField = project ? `"[[${project}]]"` : "";
-%>
---
type: note
created: <% tp.date.now("YYYY-MM-DD") %>
project: <% projectField %>
summary: 
---
