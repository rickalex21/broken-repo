---
title: AREADME
draft: false
---

## AREADME

This file was put here to show how the plugin can't handle files that contain 'README'
I think this is because the plugin uses the ```endswith```. This in turn can lead
to unexpected results.

EXPECTED: Show this file in the side bar. 

FIX: 
1. Don't use files that end in README (e.g., LATER_README.md, AREADME.md)
2. The plugin needs to be fixed with a new regex. 