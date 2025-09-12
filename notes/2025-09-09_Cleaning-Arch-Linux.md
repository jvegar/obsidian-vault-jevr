---
date: 2025-09-09
tags:
  - code-snip
  - #archlinux

hubs:
  - [[archlinux]]
    
urls:
  -
---
# 2025-09-09_Cleaning-Arch-Linux

## Stage 1: Cleaning inside Arch Linux

1. Clean the pacman cache
- To remove all cached packages that are no longer installed
and all old versions of currently installed packages.

```sh
sudo pacman -Sc
```

- For a more aggresive cleanup that remove all cached packages.

```sh
sudo pacman -Scc
```

2. Remove orphaned dependencies
- List all orphaned packages to see what will be removed

```sh
pacman -Qtdq
```

- To remove them all automatically.

```sh
sudo pacman -Rns $(pacman -Qtdq)
```

3. Cleanup large files
- List all folder sorted by disk usage

```sh
ncdu -x /
```

- Cleanup pnpm cache
```sh
pnpm store prune
```

- Cleanup go cache
```sh
go clean cache
go clean -modcache
go clean -testcache
```

- Delete fnm downloads cache folder
```sh
rm -rf ~/.fnm/node-versions/.downloads/*(D)
```







