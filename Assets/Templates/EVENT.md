<%*
// ###########################################################
//                        Helper Functions
// ###########################################################

// Convert string to camelCase
function toCamelCase(str) {
	return str.replace(/(?:^\w|[A-Z]|\b\w)/g, (word, index) => index === 0 ? word.toLowerCase() : word.toUpperCase()).replace(/\s+/g, '');
}

// Return icon based on type
function getIcon(type) {
	const iconMappings = {
		Personal: ':FasCalendarDays:',
		Political: ':FasBullhorn:',
		Religious: ':FasCross:',
		Seasonal: ':RiSunFoggyFill',
	};

	return iconMappings[type] || ':fas_question_circle:';
}

// ###########################################################
//                        Main Code Section
// ###########################################################

// Call modal form
const result = await MF.openForm('EVENT');

// Declare variables after form returns values
const name = result.Name.value;
let type = result.Type.value;
const icon = getIcon(type);

if (type === 'Manual') {
	type = await tp.system.prompt('Enter type:', 'Leave blank for none', true);
	type = type === 'Leave blank for none' || '' ? '' : type;
}

// Rename & open note
await tp.file.rename(name);
await app.workspace.getLeaf(true).openFile(tp.file.find_tfile(name));
new Notice().noticeEl.innerHTML = `<span style="color: green; font-weight: bold;">Finished!</span><br>New event <span style="text-decoration: underline;">${name}</span> added`;
_%>

---
type: Событие
tags:
 - <% type ? `event/${toCamelCase(type)}` : '' %>
headerLink: "[[<% name %>#<% name %>]]"
---

###### <% name %>
<span class="sub2"><% type ? `${icon} ${type} Event` : '' %></span>
___

> [!quote|no-t]
>ИЗОБРАЖЕНИЕ| ВСТАВЬ| СЮДА
>
>ОПИСАНИЕ ПИШЕМ ТУТ
<span class="clearfix"></span>

#### marker
> [!column|flex 3]
>>[!hint]- НПСы
>>```dataview
>>LIST WITHOUT ID headerLink
>FROM "Компендиум/НПСы" AND [[<% name %>]]
>
>>[!note]- История
>>```dataview
>LIST WITHOUT ID headerLink
>FROM "Сессионные Заметки" AND [[<% name %>]]