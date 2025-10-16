---
title: "{{title}}"
aliases: ["{{title}}"]
authors: <%=book.authors.map(author=>`\n  - "${author}"`).join('')%>
publisher: "{{publisher}}"
publish: "{{publishDate}}"
pages: {{totalPage}}
isbn10: "{{isbn10}}"
isbn13: "{{isbn13}}"
reviewed: false
cover: "<%=book.coverUrl ? `https://books.google.com/books/publisher/content/images/frontcover/${[...book.coverUrl.split("&")[0].matchAll(/id.?(.*)/g)][0][1]}?fife=w600-h900&source=gbs_api` : ''%>"
read_count: 0
tags: ["book", <%=book.categories.map(cat =>`"${cat.toLowerCase().replaceAll('&', 'and').replace(/\s+/g, '-')}"`).join(', ')%>]
status: "To Read"
log:
  - status: "To Read"
    timestamp: <%=window.moment().format("YYYY-MM-DDTHH:mm:ssZ") %>
created: <%=window.moment().format("YYYY-MM-DDTHH:mm:ssZ") %>
updated: <%=window.moment().format("YYYY-MM-DDTHH:mm:ssZ") %>
---

> [!info] About {{title}} by {{author}}
> <img src="<%=book.coverUrl ? `https://books.google.com/books/publisher/content/images/frontcover/${[...book.coverUrl.split("&")[0].matchAll(/id.?(.*)/g)][0][1]}?fife=w600-h900&source=gbs_api` : ''%>" style="float: left; width: 150px; height: auto; margin-right: 1em;" /> {{description}}
