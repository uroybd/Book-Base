---
title: "<% tp.file.title %>"
handle: "<% await tp.system.prompt("Handle") %>"
---
```dataviewjs
const {Formatters} = customJS;
Formatters.book_collection_table(dv);
dv.container.classList.add('cards','cards-cols-4', 'cards-cover');
```