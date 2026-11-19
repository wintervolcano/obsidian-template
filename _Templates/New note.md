<%*
// One command for every kind of note. Pick the type; the file is named,
// filed and opened for you. Bind this template to a hotkey in Templater.
const TYPES = [
  { label: "Observation",      tpl: "Observation",    folder: "04 OBSERVATIONS", date: true  },
  { label: "Processing run",   tpl: "Processing",     folder: "05 PROCESSING",   date: true  },
  { label: "Test / experiment",tpl: "Test",           folder: "06 TESTS",        date: true  },
  { label: "Project",          tpl: "Project",        folder: "01 PROJECTS",     own: true   },
  { label: "Project note",     tpl: "Project note",   folder: "01 PROJECTS",     pick: true  },
  { label: "Weekly meeting",   tpl: "Weekly meeting", self: true },
  { label: "Journal club",     tpl: "Journal club",   self: true },
  { label: "Meeting",          tpl: "Meeting",        folder: "02 MEETINGS",     date: true  },
  { label: "Talk attended",    tpl: "Talk",           folder: "10 TALKS",        date: true  },
  { label: "Lecture",          tpl: "Lecture",        folder: "11 LECTURES",     date: true  },
  { label: "Talk given",       tpl: "Talk given",     folder: "12 MY TALKS"                  },
  { label: "Conference / workshop", tpl: "Event",     folder: "13 EVENTS"                    },
  { label: "Concept",          tpl: "Concept",        folder: "07 CONCEPTS"                  },
  { label: "Method / how-to",  tpl: "Method",         folder: "08 METHODS"                   },
  { label: "Writing",          tpl: "Writing",        folder: "09 WRITING"                   },
  { label: "Person",           tpl: "Person",         folder: "14 PEOPLE"                    },
  { label: "Note (admin, rough, anything)", tpl: "Note", folder: "18 MISC",       where: true  },
  { label: "Teaching session",  tpl: "Teaching session", folder: "11 LECTURES", course: true, date: true },
  { label: "Issue / fix",      tpl: "Issue",          folder: "19 ISSUES"                    },
  { label: "Pulsar candidate", tpl: "Candidate",      folder: "20 CANDIDATES"                },
  { label: "Cluster run",      tpl: "Run",            folder: "21 RUNS",         date: true  },
];

// folders a plain note can land in
const NOTE_FOLDERS = ["18 MISC", "09 WRITING", "07 CONCEPTS", "08 METHODS"];

const c = await tp.system.suggester(t => t.label, TYPES, false, "What kind of note?");
if (!c) return;

const tplFile = tp.file.find_tfile(c.tpl);
if (!tplFile) { new Notice(`Template "${c.tpl}" not found in _Templates.`); return; }

// Templates that name and file themselves
if (c.self) { await tp.file.create_new(tplFile, "Untitled", true); return; }

const name = await tp.system.prompt(`${c.label} -- name:`);
if (!name) return;

let folder = c.folder;

if (c.pick) {                                   // a small note inside a project
  const dirs = app.vault.getAllLoadedFiles()
    .filter(f => f.children && f.path.startsWith("01 PROJECTS/")
                 && f.path.split("/").length === 2)
    .map(f => f.path).sort();
  if (dirs.length) {
    const pick = await tp.system.suggester(p => p.replace("01 PROJECTS/",""), dirs,
                                           false, "Which project?");
    if (pick) { folder = pick; window._tpProject = pick.replace("01 PROJECTS/", ""); }
  }
}

if (c.own) folder = `01 PROJECTS/${name}`;      // a project gets its own folder

if (c.course) {                                 // a teaching session: pick the course folder
  const courses = app.vault.getAllLoadedFiles()
    .filter(f => f.children && f.path.startsWith("11 LECTURES/")
                 && f.path.split("/").length === 2)
    .map(f => f.path).sort();
  if (courses.length) {
    const pick = await tp.system.suggester(p => p.replace("11 LECTURES/", ""), courses,
                                           false, "Which course?");
    if (pick) { folder = pick; window._tpCourse = pick.replace("11 LECTURES/", ""); }
  }
}

if (c.where) {                                  // a plain note: ask where it belongs
  const pick = await tp.system.suggester(f => f, NOTE_FOLDERS, false, "Put it where?");
  if (pick) folder = pick;
}

const fname = c.date ? `${tp.date.now("YYYY.MM.DD")} ${name}` : name;
await tp.file.create_new(tplFile, fname, true, folder);
%>
