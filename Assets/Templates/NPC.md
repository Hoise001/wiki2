---
dg-home: false
dg-publish: true
---
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
const result = await MF.openForm('NPC');

// Declare variables after form returns values
const affinity = result.Affinity.value;
const gender = result.Gender.value;
const job = result.Job.value;
const location = result.Location.value;
const location2 = result.Job.value;
const location3 = result.Events.value;
const location4 = result.God.value;
const location5 = result.Item.value;
const name = result.Name.value;
const race = result.Race.value;

// Rename & open note
await tp.file.rename(name);
await app.workspace.getLeaf(true).openFile(tp.file.find_tfile(name));
new Notice().noticeEl.innerHTML = `<span style="color: green; font-weight: bold;">Finished!</span><br>New NPC <span style="text-decoration: underline;">${name}</span> added`;
_%>

---
type: НПС
locations:
 - <% location ? `"[[${location}]]"` : '' %>
 - <% location2 ? `"[[${location2}]]"` : '' %>
 - <% location3 ? `"[[${location3}]]"` : '' %>
 - <% location4 ? `"[[${location2}]]"` : '' %>
 - <% location5 ? `"[[${location2}]]"` : '' %>
tags:
 - race/<% toCamelCase(race) %>
 - affinity/<% toCamelCase(affinity) %><% job ? `\n - job/${toCamelCase(job)}` : '' %>
headerLink: "[[<% name %>#<% name %>]]"
---
###### <% name %>
<span class="sub2"><% location ? `:FasMapLocationDot: [[${location}#${location}]] &nbsp;|&nbsp; ` : '' %>:FasHeartPulse: <% affinity %> </span>
___

> [!infobox|no-t right]
>
> ИЗОБРАЖЕНИЕ| ВСТАВЬ| СЮДА
>
>
> ###### Подробности:
> | Type | Stat |
> | ---- | ---- |
> | :FasBriefcase: Род Занятий |  <% job %> |
> | :FasVenusMars: Пол | <% gender %> |
> | :FasUser: Раса | <% race %> |
> | :FasUser: Раса | <% race %> |
<span class="clearfix"></span>

> [!quote|no-t]
>ОПИСАНИЕ ПИШЕМ ТУТ

#### marker
> [!column|flex 3]
>> [!important]- Квесты:
>>```dataview
>>LIST WITHOUT ID headerLink
>>FROM "Компендиум/Партия/Квесты" AND [[<% name %>]]
>
>>[!note]- История
>>```dataview
>>LIST WITHOUT ID headerLink
>>FROM "Сессионные Заметки" AND [[<% name %>]]
