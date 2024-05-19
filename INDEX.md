---
cssClasses: index
dg-publish: true
dg-home: true
---
![[Compendium.jpg|banner]]
###### <span class="head">Журнал Компании</span> 

```dataviewjs
// Set path and class
const vault = this.app.vault.adapter.getResourcePath("").split("?")[0];
dv.container.className += ' hideSort cards cards-cover cards-1-1';

// Display PC card table
dv.table(["cover", "name", "details"],
  dv.pages(`"Компендиум/Партия/Персонажи"`)
  .sort(page => page.file.name, "asc")
    .map(p => [
      `![](${vault}/${p.cover})`, // For image link use: [![](${vault}/${p.cover})](<${p.file.name}#${p.file.name}>)
      p.headerLink,
      obsidian.Platform.isMobile ? `:FasCrown: Level ${p.level}<br>:FasUserGroup: ${p.race}<br>:RiSwordFill: ${p.class}` : `:FasCrown: Level ${p.level} / :FasUserGroup: ${p.race} / :RiSwordFill: ${p.class}`
    ])
);
```

> [!npc]-   НПСы<br><span class="sub">Не-играбельные Персонажи</span>
> ```dataviewjs
> dv.container.className += ' listMe';
> let pages = dv.pages('"Компендиум/НПСы"').sort(p => p.file.name, "asc");  
> dv.table(["Name"], pages.map(page => [`- ${page.headerLink}`]));
>```
> `BUTTON[npc]`

> [!agenda]-  Задания<br><span class="sub">Задачи & Квесты</span>
>```dataviewjs
>dv.container.className += ' listMe';
>let tags = [];
>let pages = dv.pages('"Компендиум/Партия/Квесты"').sort(p => p.file.name, "asc");
>dv.table(["Name", "Status"], pages.map(page => {
>const questStatusTerms = ["quest/completed", "quest/abandoned", "quest/failed", "quest/ongoing", "quest/pending"];
>let status = questStatusTerms.find(term => page.tags && page.tags.includes(term));
>status = status ? `(${status.replace("quest/", "")})` : "";
>return [`- ${page.headerLink} ${status}`, status];
>}));
>```
> `BUTTON[quest]`

> [!genloc]-  Локации<br><span class="sub">Страны, Поселения & Топография</span>
> ```dataviewjs
> dv.container.className += ' listMe';
> let pages = dv.pages('"Компендиум/Атлас"').sort(p => p.file.name, "asc");  
> dv.table(["Name", "Type"], pages.map(page => [`- ${page.headerLink} (${page.type})`, page.type]));
>```
>`BUTTON[plane, realm, continent, region, locale, landmark]`

> [!lore]-  Лор & Мифы<br><span class="sub">Фракции, боги, объекты, предметы и т.д.</span> 
> ```dataviewjs
> dv.container.className += ' listMe';
> let pages = dv.pages('"Компендиум/Лор"').sort(p => p.file.name, "asc");  
>dv.table(["Name", "Type"], pages.map(page => [`- ${page.headerLink} (${page.type})`, page.type]));
>```
> `BUTTON[deity, event, object, org]`
 
> [!session]-  Сессионные Заметки<br><span class="sub">Итоги, транскрипты диалогов и заметки</span>
> ```dataviewjs
> dv.container.className += ' listMe';
> let pages = dv.pages('"Сессионные Заметки"').sort(p => p.file.name, "desc");  
>dv.table(["Date"], pages.map(page => [`- ${page.headerLink}`]));
>```
> `BUTTON[note]`