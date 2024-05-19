<%*
// ###########################################################
//                        Helper Functions
// ###########################################################

// Return icon based on type
function getIcon(type) {
	const iconMappings = {
		Continent: ':FasEarthAmericas:',
		Ocean: ":FasWater:",
	};

	return iconMappings[type] || ':fas_question:';
}

// Return modified path based on location
const dv = app.plugins.plugins.dataview.api;
function getPath(location) {
	const match = dv.pages('"Компендиум/Атлас"')
		.where(p => p.type === 'realm' && p.file.name === location)
		.map(obj => obj.file.path.split('/').slice(2, -1).join('/'))
		.find(Boolean);

	return match || '';
}

// ###########################################################
//                        Main Code Section
// ###########################################################

// Call modal form
const result = await MF.openForm('CONTINENT');

// Declare variables after form returns values
const location = result.Location.value;
const name = result.Name.value;
const type = result.Type.value;
const icon = getIcon(type);
const path = getPath(location);

// Rename, move, & open note
await tp.file.move(`Компендиум/Атлас/${location ? `${path}/` : ''}${name}/${name}`);
await app.workspace.getLeaf(true).openFile(tp.file.find_tfile(name));
new Notice().noticeEl.innerHTML = `<span style="color: green; font-weight: bold;">Finished!</span><br>New ${type.toLowerCase()} <span style="text-decoration: underline;">${name}</span> added`;
_%>

---
type: continent
locations:
 - <% location ? `"[[${location}]]"` : '' %>
tags:
- 
headerLink: "[[<% name %>#<% name %>]]"
---

###### <% name %>
<span class="sub2"><% icon %> <% type %></span>
___

> [!quote|no-t]
> ИЗОБРАЖЕНИЕ| ВСТАВЬ| СЮДА
>ОПИСАНИЕ ПИШЕМ ТУТ

#### marker
> [!column|flex 3]
> > [!hint]-  НПСы
> > <input type="checkbox" id="npc"/><ul class="sortMenu"><li class="sortIcon">:RiListSettingsLine:<ul class="dropdown npcedit"><li><label for="npc" class="directLabel active">Direct Links Only</label></li><li><label for="npc" class="childLabel">Include Sub-Locations</label></li></ul></li></ul>
> >```dataviewjs
dv.container.className += ' npcDirect';
dv.list(dv.pages('"Компендиум/НПСы"')
 .where(p => p.file.outlinks.includes(dv.current().file.link))
.sort(p => p.file.link)
.map(p => p.headerLink), 1);
>>```
>>```dataviewjs
dv.container.className += ' npcChild';
let page = dv.current().file.path;
let pages = new Set();
let stack = [page];
while (stack.length > 0) {
let elem = stack.pop();
let meta = dv.page(elem);
if (!meta) continue;
for (let inlink of meta.file.inlinks.concat(meta.file.outlinks).array()) {
let locations = dv.page(inlink.path);
if (!locations || pages.has(inlink.path) || inlink.path === meta.locations?.[0]) continue;
 if (dv.array(locations.locations).join(", ").includes(meta.file.path)) {
 pages.add(inlink.path);
 stack.push(inlink.path);
}}}
let data = Array.from(pages)
.filter(p => dv.page(p)?.type === "npc")
.map(p => dv.page(p).headerLink)
.sort((a, b) => {
if (a < b) return -1;
if (a > b) return 1;
return 0;
});
dv.list(data);
> 
>> [!example]- Локации
>>```dataview
LIST WITHOUT ID headerLink
FROM "Компендиум/Атлас/<% location ? `${path}/` : '' %><% name %>"
WHERE type= "region"
SORT file.name ASC
>
>> [!note]- История
>>```dataview
LIST WITHOUT ID headerLink
FROM "Сессионные Заметки" AND [[<% name %>]]
SORT file.ctime DESC
#### marker