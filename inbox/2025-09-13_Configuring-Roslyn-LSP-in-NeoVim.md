---
date: 2025-09-13
tags:
  - lsp
hubs:
  - "[[roslyn]]"
urls:
  - https://github.com/dotnet/roslyn
---
# 2025-09-13_Configuring-Roslyn-LSP-in-NeoVim
These are the steps for configuring Roslyn LSP in NeoVim:

## Installing RosLyn LSP server
Add the following registries to your Mason config:
```json
    registries = {
        "github:mason-org/mason-registry",
        "github:Crashdummyy/mason-registry",
    },
```

This will enable roslyn to be listed on Mason. Then you can just use: `:MasonInstall roslyn`


