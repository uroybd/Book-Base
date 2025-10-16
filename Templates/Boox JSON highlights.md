<%*
const NOTE_STYLES = {
  lemon: {
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
  chartreuse: {
    type: "sceptic",
    title: "Sceptic"
  },
  orange: {
    type: "warning",
    title: "Unsound"
  },
  violet: {
    type: "stylish",
    title: "Stylish"
  }
};

const COLOR_DICT = {
  "#000000": "black",
  "#404040": "dark-grey",
  "#808080": "grey",
  "#c0c0c0": "light-grey",
  "#ffffff": "white",
  "#ffc1c3": "red",
  "#00b036": "green",
  "#000084": "blue",
  "#00f0ff": "aqua",
  "#ee00ff": "violet",
  "#ffaa00": "orange",
  "#f0ff00": "lemon",
  "#008000": "chartreuse",
  "#9338be": "grape",
  "#00aaff": "sky-blue",
  "#ff4400": "orange-red"
}

function get_note_style(note) {
    const colorName = COLOR_DICT[note.color]
    if (colorName == undefined) {
        return NOTE_STYLES["lemon"]
    }
    const style = NOTE_STYLES[colorName]
    if (style == undefined) {
        return NOTE_STYLES["lemon"]
    }
    return style
}

function format_percentages(page, total) {
	if (page && total) {
		return `${((page / total) * 100).toFixed(2)}%`;
	}
	return "";
}

function format_quote(text, keepBreak) {
    if (!keepBreak) {
        return text.replaceAll(/\n\n/g, '__temp__').replaceAll(/\n/g, ' ').replaceAll('__temp__', '\n');
    }
    return text
}

async function format_json_highlights(content) {
	const data = JSON.parse(content);
    let keepBreak = false
	if (data.format == "pdf") {
    	keepBreak = await tp.system.suggester(["No", "Yes"], [false, true], false, "Keep line break?")
	}
    console.log("keepBreak: ", keepBreak)
    let output = `---\ntitle: "${data.title}"\naliases: ["Notes from ${data.title}"]\nauthor: "${data.authors}"\n---\n# ${data.title}\n##### ${data.authors}`;

	let current_chapter = "";
	for (const entry of data.annotations) {
	  if (entry.quote) {
		if (entry.chapter != current_chapter) {
		  output += `\n\n## ${entry.chapter}\n`;
		  current_chapter = entry.chapter;
		}
		output += `\n### Page: ${
		  entry.pageNumber
		} (${format_percentages(
		  entry.pageNumber,
		  data.pageNumber
		)}) @ ${window.moment(entry.createdAt).format("DD MMM YYYY hh:mm:ss A")}\n`;
		output += `\n${format_quote(entry.quote, keepBreak)}`;
		const note_type = get_note_style(entry);
		output += `\n\n> [!${note_type.type}] ${note_type.title}`;
		if (entry.note) {
		  if (entry.note.length > 50) {
			output += `\n> ${entry.note.replaceAll("\n", "\n> ")}`;
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
const output = await format_json_highlights(content);
let file = app.workspace.getActiveFile();
await app.vault.modify(file, output);
%>
