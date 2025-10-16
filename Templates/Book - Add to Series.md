<%*  
const path = "Personal/Reading/Series";

const filteredFiles = app.vault.getMarkdownFiles().filter(file => {  
return file.path.startsWith(path);  
});  
const selectedFile = await tp.system.suggester((file) => file.basename, filteredFiles);
const position = await tp.system.prompt("Position");
const handle = await app.fileManager.processFrontMatter(selectedFile, (series) => {
  return series.handle;
})
if (handle) {
const currentFile = app.workspace.getActiveFile();

await app.fileManager.processFrontMatter(currentFile, async (cr) => {
  console.log(handle, position);
      if (!cr.series) {
        cr.series = {} 
      };
      cr.series[handle] = position
      console.log(cr);
});
}
-%>