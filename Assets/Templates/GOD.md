<%*
// ###########################################################
//                        Helper Functions
// ###########################################################

// Convert string to camelCase
function toCamelCase(str) {
	return str.replace(/(?:^\w|[A-Z]|\b\w)/g, (word, index) => index === 0 ? word.toLowerCase() : word.toUpperCase()).replace(/\s+/g, '');
}

// ###########################################################
//                        Main Code Section
// ###########################################################

// Call modal form
const result = await MF.openForm('GOD');

// Declare variables after form returns values
const alignment = result.Alignment.value;
const name = result.Name.value;
const gender = result.Gender.value;
const domains = result.Domains.value;
const pantheon = result.Pantheon.value;
const tags = domains ? domains.map(value => `- domain/${toCamelCase(value)}`).join("\n") : '-';

// Rename & open note
await tp.file.rename(name);
await app.workspace.getLeaf(true).openFile(tp.file.find_tfile(name));
new Notice().noticeEl.innerHTML = `<span style="color: green; font-weight: bold;">Finished!</span><br>New deity <span style="text-decoration: underline;">${name}</span> added`;
_%>

---
type: Божество
tags:
<% tags %>
headerLink: "[[<% name %>#<% name %>]]"
---

###### <% name %>
<span class="sub2">:FasCross: *Божество* &nbsp; | &nbsp; :FasYinYang: <% alignment %></span>
___

> [!infobox|no-t right]
> ИЗОБРАЖЕНИЕ| ВСТАВЬ| СЮДА
> ###### Подробности:
> | Type | Stat |
> | ---- | ---- |
> | :FasBoltLightning: Владения | <% domains.join(', ') %> |
> | :FasVenusMars: Пол | <% gender %> |
> | :FasBuildingColumns: Пантеон | <% pantheon %> |

> [!quote|no-t]
>ОПИСАНИЕ ПИШЕМ ТУТ

#### marker
> [!column|flex 3]
>> [!hint]-  НПСы
>>```dataview
>>LIST WITHOUT ID headerLink
>>FROM "Компендиум/НПСы" AND [[<% name %>]] 
>
>>[!note]- История
>>```dataview
>>LIST WITHOUT ID headerLink
>>FROM "Сессионные Заметки" AND [[<% name %>]]