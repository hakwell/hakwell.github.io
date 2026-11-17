# Importing Markdown posts

Writeups and Christianity articles are Markdown files. Add your own files to the folders below; the homepage lists them automatically and each title opens a full article page using the site's matching colors and typography.

- Cybersecurity writeups: `_writeups/`
- Christianity and faith posts: `_faith/`

No example posts or writeups are included. Keep filenames short and use lowercase letters, numbers, and hyphens, for example `linux-notes.md` or `faith-and-purpose.md`.

## Writeup template

Save a `.md` file under `_writeups/`. Set `platform` to one of `HackTheBox`, `OffSec Labs`, `TryHackMe`, `HackSmarter`, or `Tracehunt`. The homepage filter uses this value.

```markdown
---
layout: article
title: "Your writeup title"
date: 2026-01-31
platform: "HackTheBox"
excerpt: "A short summary shown on the homepage."
---

## Overview

Paste or write your Markdown content here.

## Notes

Continue with regular Markdown headings, lists, links, images, and fenced code blocks.
```

## Faith post template

Save a `.md` file under `_faith/`. Use `author` to credit yourself or a guest contributor. Only publish another person's writing after they have given permission and approved the version being shared.

```markdown
---
layout: article
title: "Your post title"
date: 2026-01-31
author: "Isezerano Brice Fiquerite"
excerpt: "A short summary shown on the homepage."
---

## Your first section

Paste or write your article in Markdown.
```

## Notion export notes

Export or copy the Notion page as Markdown, then place the `.md` file in the correct collection folder. Keep the metadata block at the very top of the file, then put the Notion body below it. Move any downloaded image or attachment files into the repository and update their Markdown links so the images resolve on the published site.
