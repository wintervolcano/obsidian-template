<%*
let course = window._tpCourse || "";
window._tpCourse = null;
if (!course) {
  const m = (tp.file.folder(true) || "").match(/^11 LECTURES\/([^\/]+)/);
  if (m) course = m[1];
}
const courseField = course ? `"[[${course}]]"` : "";
-%>
---
type: lecture
role: tutor
created: <% tp.date.now("YYYY-MM-DD") %>
date: <% tp.date.now("YYYY-MM-DD") %>
event: <% courseField %>
presenter: 
session: 
status: attended
---
## Lecture


## Tutorial

> [!method] To prepare
> 

> [!limitation] Where they'll get stuck
> 

### Exercises
- [x]  ✅ 2026-10-16

### Student questions

> [!question] #q
> 

## Afterwards

> [!result] What worked
> 

> [!limitation] Fix next time
> 
