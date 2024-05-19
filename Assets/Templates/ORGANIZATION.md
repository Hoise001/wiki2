---
dg-home: false
dg-publish: true
---
<%*
// ###########################################################
//                        Main Code Section
// ###########################################################

// Call modal form
const result = await MF.openForm('ORGANIZATION');

// Declare variables after form returns values
const alignment = result.Alignment.value;
const name = result.Name.value;
const location = result.Location.value;

// Rename & open note
await tp.file.rename(name);
await app.workspace.getLeaf(true).openFile(tp.file.find_tfile(name));
new Notice().noticeEl.innerHTML = `<span style="color: green; font-weight: bold;">Finished!</span><br>New organization <span style="text-decoration: underline;">${name}</span> added`;
_%>

---
type: Организация
locations:
 - <% location ? `"[[${location}]]"` : '' %>
tags:
- 
headerLink: "[[<% name %>#<% name %>]]"
---

###### <% name %>
<span class="sub2">:FasSitemap: Organization</span>
___

> [!quote|no-t]
>!ИЗОБРАЖЕНИЕ| ВСТАВЬ| СЮДА| 
>ОПИСАНИЕ ПИШЕМ ТУТ

#### marker
> [!column|flex 3]
>>[!hint]- НПСы
>>```dataview
>>LIST WITHOUT ID headerLink
>>FROM "Компендиум/НПСы" AND [[<% name %>]]
>
>>[!note]- История
>>```dataview
>>LIST WITHOUT ID headerLink
>>FROM "Сессионные Заметки" AND [[<% name %>]]