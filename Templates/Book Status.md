<%*
let status = await tp.system.suggester((item) => item, ['To Read', 'In Progress', 'Read', 'Abandoned'], false, 'status');
let file = app.workspace.getActiveFile();
let alreadyRead = null;
await app.fileManager.processFrontMatter(file, (fm) => {
	if (fm.log == undefined) {
		fm.log = []
	}
	fm.log.unshift({status: status, timestamp: tp.date.now("YYYY-MM-DDTHH:mm:ssZ")})
	fm.read_count = fm.read_count || 0
	alreadyRead = fm.read_count > 0
	fm.read_count = fm.log.filter((l) => l.status == "Read").length;
	fm.status = status;
});
if (alreadyRead) {
app.fileManager.renameFile(file, `Personal/Reading/Books/Read/${file.name}`);
} else {
app.fileManager.renameFile(file, `Personal/Reading/Books/${status}/${file.name}`);
}
%>