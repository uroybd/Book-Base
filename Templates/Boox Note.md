<%* 
const {BooxFormatter} = await cJS()


let file = app.workspace.getActiveFile()
const content = await tp.system.prompt("Paste the JSON content", null, true, true);
let total_pages = await tp.system.prompt("Number of total pages");
total_pages = parseInt(total_pages);

await app.vault.modify(file, BooxFormatter.formatFromString(content, total_pages))
%>