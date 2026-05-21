[中文](README_zh.md) | [English](README.md)

# RenPy Tutorial Translation Skill

Codex skill for working on Ren'Py game translations according to the bundled Chinese tutorial:

- [Renpy游戏翻译教程V2.0.docx](references/Renpy游戏翻译教程V2.0.docx)
- [Compact tutorial notes](references/tutorial-notes.md)

## What It Covers

- Generating and updating Ren'Py translation files.
- Working with `translate <language> ...` dialogue blocks.
- Working with `translate <language> strings`, `old`, and `new`.
- Finding strings Ren'Py may not extract automatically.
- Adding language switching.
- Fixing common translation issues such as overflow, missing glyphs, image text, interpolation, and concatenated strings.
- Creating basic and advanced translation patches, including `zzz.rpy`, `init offset`, and label overrides.

## Install

Clone or copy this folder into your Codex skills directory:

```powershell
git clone https://github.com/September6969/renpy-tutorial-translation.git "$env:USERPROFILE\.codex\skills\renpy-tutorial-translation"
```

Then start a new Codex session so the skill can be discovered.

## Use

Ask Codex to use `$renpy-tutorial-translation` when working on a Ren'Py translation task.

Example:

```text
Use $renpy-tutorial-translation to check this Ren'Py translation patch against the tutorial.
```

## Contents

```text
SKILL.md
agents/openai.yaml
references/tutorial-notes.md
references/Renpy游戏翻译教程V2.0.docx
```
