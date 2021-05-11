---
title: Code
draft: False
---

## Readme

I have included the problems here. If you want to fix the repo do this.

1. Rename index.md to README.md, this is the home page.
2. Rename AREADME.md to AREADMEB.md.
3. Add ```draft: false``` to lua.md to show the side bar for code/

## Problems

The sidebar should show python but not lua. Instead the sidebar does not show anything
when a draft is set to false on one of them.

1. Setting files frontmatter to ```draft: true``` will make all the items in code/
disappear from the sidebar. Expected behavior is to show the ```draft: false``` items
but not the ```draft: true```.

For example, when you set lua.md to ```draft: true``` it will make python.md and
everything else in the code sidebar disappear. 

I filter the frontmatter in index.js with this code and it works as it should but
the side bar does not handle it properly. You can see that python is there but not
[lua](http://localhost:8080/code/lua) which is the expected behavior but that
does not reflect in the sidebar. 

EXPECTED: Show all code/ items in the sidebar except for lua.md

FIX: Don't use drafts, this would be a problem because everyone filters
the frontmatter. Plugin needs to be fixed.

```javascript
async ready(){
        ctx.pages.splice(
          0,
          ctx.pages.length,
          ...ctx.pages.filter(({ frontmatter,path }) => {
              let title = frontmatter.title
              let draft = frontmatter.draft
              console.log('frontmatter.draft is ',draft, title,path );
             return  draft !== true
            }),
        )
},
```
2. Adding a file that ends with README.md like AREADME.md will cause an error. This is
because the plugin uses ```endswith``` string method instead of using a RegExpr? 

EXPECTED: Show this file AREADME.md in the sidebar. 

FIX: 
* Don't use files that end in README (e.g., LATER_README.md, AREADME.md). Rename
AREADME to AREADMEB.md to avoid the problem. 
* The plugin needs to be fixed with a new regex. 

Maybe try something like.

```javascript
const regex = new RegExp('^readme$','i');
```

::: danger ERROR
[vuepress] No matching page found for sidebar item "/code/A"
:::

3. When installing the plugin the home page index.md must be renamed to README.md or
you will get this error. I'm not sure if the plugin is accounting for index.md files.
In vuepress pages can be named index.md or readme.md.

::: danger  ERROR
[vuepress] No matching page found for sidebar item "/index"

TypeError: path is undefined
:::

EXPECTED: Home page to show when it's named index.md which is the default.

FIX: Renaming index.md to README.md fixes this problem or fix the plugin.

This is mostly a configuration question: I wonder if there's a way that I can show
only the sidebar for the current directory and not the entire website. Not sure
if this has something to do with with the sidebar or modifying the code in
the components theme for Sidebar.vue ? 

For example, right now it looks like this.

```text
|--Code
   |- code1.md
   |- code2.md
   |--Misc
      |- misc1.md
      |- misc2.md
```

When I click on ```http://localhost:8080/code/misc/``` I would like it like it to
look like this. Like that there's not a lot of clutter with the entire site in the nav
bar.

```text
|--Misc
    |- misc1.md
    |- misc2.md
```

