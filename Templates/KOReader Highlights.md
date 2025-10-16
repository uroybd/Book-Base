<%*
const NOTE_STYLES = {
  yellow: {
	type: "quote",
	title: "Quotable/Concept/General Idea",
  },
  blue: {
	type: "important",
	title: "Striking/Intense",
  },
  red: {
	type: "danger",
	title: "In Discord",
  },
  green: {
	type: "question",
	title: "Thought Provoking",
  },
  olive: {
    type: "sceptic",
    title: "Sceptic"
  },
  orange: {
    type: "warning",
    title: "Unsound"
  },
  purple: {
    type: "stylish",
    title: "Stylish"
  }
};

function format_koreader_percentages(page, total) {
	if (page && total) {
		return `${((page / total) * 100).toFixed(2)}%`;
	}
	return "";
}

function format_koreader_json_highlights(content) {
	const data = JSON.parse(content);
    let output = `---\ntitle: "${data.title}"\naliases: ["Notes from ${data.title}"]\nauthor: "${data.author}"\n---\n# ${data.title}\n##### ${data.author}`;

	let current_chapter = "";
	for (const entry of data.entries) {
	  if (entry.text) {
		if (entry.chapter != current_chapter) {
		  output += `\n\n## ${entry.chapter}\n`;
		  current_chapter = entry.chapter;
		}
		output += `\n### Page: ${
		  entry.page
		} (${format_koreader_percentages(
		  entry.page,
		  data.number_of_pages
		)}) @ ${window.moment.unix(entry.time).format("DD MMM YYYY hh:mm:ss A")}\n`;
		output += `\n${entry.text}`;
		const note_type = NOTE_STYLES[entry.color];
		output += `\n\n> [!${note_type.type}] ${note_type.title}`;
		if (entry.note) {
		  if (entry.note.length > 50) {
			output += `\n> ${entry.note}`;
		  } else {
			output += `: ${entry.note}`;
		  }
		  output += "\n";
		}
		output += "\n";
	  }
	}
	return output;
}

const content = await tp.system.prompt("Paste the JSON content", null, true, true);
const output = format_koreader_json_highlights(content);
let file = app.workspace.getActiveFile();
await app.vault.modify(file, output);
%>
