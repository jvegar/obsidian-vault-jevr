---
date: 2025-09-13
tags:
  -
hubs:
  - "[[]]"
urls:
  -
---
# 2025-09-13_Fixing-diagnostic-errors-in-CS-files

## Description
When using NeoVim with roslyn LSP for editing cs files, some diagnostics errors appears.

## Cause
This error occurs when Roslyn language server can't find the Library assemblies.

## Solution
- Restore packages
```sh
dotnet restore
```
- Restart NeoVim
