<%*
const currentFile = app.workspace.getActiveFile();

if (!currentFile) {
  new Notice("❌ You must run this inside an open file.");
  return;
}

const today = tp.date.now("YYYY-MM-DD");
const header = `## ${today}\n`;

const content = await app.vault.read(currentFile);
const lines = content.split("\n");

// Find all daily headings and their line numbers
const headingRegex = /^## (\d{4}-\d{2}-\d{2})/;
let headings = [];
for (let i = 0; i < lines.length; i++) {
  const match = lines[i].match(headingRegex);
  if (match) {
    headings.push({ date: match[1], index: i });
  }
}

// Find the previous day's heading
let prevHeadingIndex = -1;
let prevHeadingEnd = lines.length;
for (let i = headings.length - 1; i >= 0; i--) {
  if (headings[i].date < today) {
    prevHeadingIndex = headings[i].index;
    prevHeadingEnd = (i + 1 < headings.length) ? headings[i + 1].index : lines.length;
    break;
  }
}

// Collect - [>] postponed tasks from the previous day's section
let postponed = [];
if (prevHeadingIndex !== -1) {
  for (let i = prevHeadingIndex + 1; i < prevHeadingEnd; i++) {
    const line = lines[i];
    if (line.trim().startsWith("- [>]")) {
      postponed.push(line.replace("- [>]", "- [ ]"));
    }
  }
}

// Create new daily section
let newSection = `${header}`;
newSection += postponed.length
  ? postponed.join("\n") + "\n\n"
  : "_No postponed tasks from yesterday._\n\n";

// Add vault-wide open tasks query
newSection += "### All Vault Tasks\n";
newSection += "```tasks\nnot done\n```\n\n";

// Prepend the new section to the file
const newContent = newSection + content;
await app.vault.modify(currentFile, newContent);

new Notice("✅ Daily section added at the top.");
%>
