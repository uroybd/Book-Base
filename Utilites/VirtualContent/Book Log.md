```dataviewjs
const BOOK_LOG_STYLES = {
      "To Read": {
        style: {
          color: "var(--color-yellow)",
          fontWeight: "bold"
        },
        icon: "lucide-book-plus"
      },
      "In Progress": {
        style: {
          color: "var(--color-blue)",
          fontWeight: "bold"
        },
        icon: "lucide-book-open"
      },
      "Read": {
        style: {
          color: "var(--color-green)",
          fontWeight: "bold"
        },
        icon: "lucide-book-check"
      },
      "Abandoned": {
        style: {
          color: "var(--color-red)",
          fontWeight: "bold"
        },
        icon: "lucide-book-x"
      }
    }
const current = dv.current()
dv.el("h3", "Reading Log");
current.log.forEach(function(l, idx) {
	let styles = BOOK_LOG_STYLES[l.status]
	dv.paragraph(`<span class="icon"></span> <span class="status">${l.status}</span> <small><i>${dv.date(l.timestamp).toFormat("dd MMM yyyy")}</i></small>`, {attr: {id: `rlog-${idx}`}});
    let iconEl = dv.container.querySelector(`#rlog-${idx} .icon`)
    let statusEl = dv.container.querySelector(`#rlog-${idx} .status`)
    obsidian.setIcon(iconEl, styles["icon"], 16)
    Object.keys(styles.style).forEach(function (key) {
        statusEl.style[key] = styles.style[key]
    });
});
```