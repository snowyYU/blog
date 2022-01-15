---
title: pnpm
date: 2021-12-25 11:51:30
tags: [npm, pnpm]
categories: [npm, pnpm]
---

## pnpm create

```bash
Usage: pnpm create <name>
       pnpm create <name-without-create>
       pnpm create <@scope>

Creates a project from a `create-*` starter kit.

Visit https://pnpm.io/6.x/cli/create for documentation about this command.
```

此命令相当于 `npm init` 命令

```bash
Create a package.json file

Usage:
npm init [--force|-f|--yes|-y|--scope]
npm init <@scope> (same as `npx <@scope>/create`)
npm init [<@scope>/]<name> (same as `npx [<@scope>/]create-<name>`)

Options:
[-y|--yes] [-f|--force]
[-w|--workspace <workspace-name> [-w|--workspace <workspace-name> ...]]
[-ws|--workspaces] [--include-workspace-root]

aliases: create, innit

Run "npm help init" for more info
```

### npx 命令

> [npx 使用](https://www.ruanyifeng.com/blog/2019/02/npx.html)
