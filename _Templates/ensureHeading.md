<%*
const today = tp.date.now("YYYY-MM-DD");
const heading = `## ${today}`;
const content = await tp.file.content;
if (!content.includes(heading)) {
  await tp.file.append("\n" + heading + "\n");
}
// No insertion at cursor from this template
tR = "";
_%>