# Tutorial Notes

This is a compact operational index for [Renpy游戏翻译教程V2.0.docx](Renpy游戏翻译教程V2.0.docx). It summarizes the tutorial's technical workflow without adding project-specific preferences.

## Ten-Step Route

1. Ensure `game/` contains `.rpy` scripts; unpack `.rpa` or decompile `.rpyc` when needed.
2. For a game update, bring forward old translation files before translating new material.
3. Inspect scripts for strings Ren'Py may not auto-detect.
4. Generate translation files with Ren'Py and a stable language identifier.
5. Add a language switch to the preferences screen.
6. Translate every `.rpy` under `game/tl/<language>/`.
7. Test the game for startup errors, text overflow, fonts, untranslated images, context collisions, untranslated variables, and missing strings.
8. Use fallback techniques only when normal translation mechanisms cannot solve a string.
9. Package the translation as a patch.
10. Prefer a clean patch structure that can be removed without damaging the original game.

## Core Principles

- Use spaces for indentation, not tabs.
- Strings must remain properly quoted.
- Do not delete original `.rpyc` files.
- Ren'Py language identifiers are case-sensitive.
- Any change to the original dialogue text can change its translation hash.
- On Windows, write translation files through UTF-8-safe tools. PowerShell's active code page can corrupt non-ASCII text if a command writes text without explicit UTF-8 handling.

## Translation Forms

Dialogue block:

```renpy
translate chinese start_ea0fb88e:
    # a "Hi!"
    a "Translated line."
```

String table:

```renpy
translate chinese strings:
    old "Language"
    new "语言"
```

Dialogue block translations are tied to label and hash. String-table translations are direct `old` to `new` replacements and may apply globally to identical source strings.

Menu captions are also exact string matches. Preserve the original `old` value exactly, including spaces, tags, suffixes, and condition text. If a custom screen strips a display marker before rendering, verify whether the stripped text exactly matches another translated `old`; otherwise translate the marked variant too.

Disambiguation: Ren'Py can distinguish otherwise identical visible strings with the invisible `{#...}` text tag. Use this when the source/template can be adjusted and one visible string needs multiple translations.

## Updates and Existing Translations

For a game update:

1. Download the new version.
2. Extract `.rpy` scripts if needed.
3. Copy the old version's `game/tl/<language>/` folder into the new version.
4. Generate translation files with the same language identifier.
5. Do not replace existing translations; let Ren'Py append newly untranslated strings and create files for new scripts.

For translating from an existing translation:

1. Copy `game/tl/<other-language>/` to `game/tl/<language>/`.
2. Replace `translate <other-language>` headers with `translate <language>`.
3. Translate the existing translated text into the target language while preserving hashes, `old` strings, tags, and code.

## Manual String Discovery

Search scripts for strings in:

- `menu:` option captions
- `Character("...")`
- `text "..."`
- `textbutton "..."`
- `tooltip "..."`
- `renpy.input("...")`
- `renpy.notify(...)`
- `default variable = "..."`
- `define variable = "..."`
- `$ variable = "..."`

The tutorial gives two routes: mark strings with `_()` then regenerate translations, or manually add exact `old`/`new` entries.

For menus, do not assume generated translation files are complete enough. Check the original `menu:` blocks and confirm every original caption has an exact-match translation path after all custom tags and runtime screen transformations are considered.

## Troubleshooting Coverage

Use these checks from Chapter 3:

- Errors: read `errors.txt` for generation/startup problems and `traceback.txt` for runtime errors. Look for missing quotes, tabs, bad indentation, broken `{}` text tags, bad variables, or altered special symbols.
- Encoding: if a bulk edit introduces mojibake, `????`, broken Chinese, or damaged tags, restore the affected strings and redo the edit with an explicit UTF-8 write path.
- Overflow: shorten text, split one source line into several translated dialogue lines, or adjust style/textbox settings.
- Fonts: use a font that supports the target language, preferably via style overrides in an advanced patch instead of changing original files for every language.
- Image text: translate image assets and mirror the original path structure.
- Same source, different meaning: one `old` value gives one global `new`, so solve context collisions by extracting more specific strings or using patch techniques.
- Menu exact-match: if a menu option stays in the source language, compare the runtime caption to the `old` string exactly, including spaces and text tags. Add the missing exact `old`/`new` entry if no existing base string covers it.
- Interpolation: translate variable values and use translation-aware interpolation such as `[variable!t]` for ordinary variables when needed. Use `[variable!ti]` when the translated substitution itself contains interpolation that must be evaluated.
- Grammar consistency: put conditional Ren'Py code in translation blocks when variable values require different sentence forms.
- Concatenated strings: use `__()` around translatable fragments or isolate source changes through a patch/label override.

## Three Last-Resort Techniques

The tutorial calls these final tools and warns about side effects:

1. `__()` marks text for immediate translation when stored in variables. Use carefully because stored translated values can affect later display or logic.
2. `___()` immediately translates a string and supports interpolation from the caller's scope. It can double-translate if the displayed result also matches a string translation.
3. `renpy.translate_string()` directly translates a string but does not add it to generated translation templates.
4. `config.replace_text` performs late display-time replacement. Keep replacements specific and avoid broad substitutions that might affect already translated text.
5. `preferences.language` checks can limit replacements or source-side custom code to the target language.

## Standardized Ren'Py Mechanisms

Use these mechanisms as the normal form when they fit the translation problem:

- File and image translations: place translated assets under `game/tl/<language>/` using the same path as the original asset under `game/`.
- Style translations: use `translate <language> style <style_name>:` for language-specific style changes.
- GUI variable/font changes: use `translate <language> python:` for language-specific variables such as GUI fonts when this is cleaner than overriding the whole screen.
- Deferred translation loading: `config.defer_tl_scripts = True` can reduce load time in large projects, but translation scripts that define screens or labels cannot be deferred.
- Translation info: Ren'Py exposes translation identifier/source information for debugging displayed text and mapping it back to the generated translation files.
- `_p()` can normalize multi-line paragraph strings and can be used in `new _p("""...""")` for cleaner string translations with paragraph breaks.

## Advanced Patch Pattern

Create a patch file in `game/tl/<language>/`, for example:

```renpy
init offset = 1

define config.default_language = "chinese"
```

Use this file for screen overrides, style overrides, default-language settings, and fallback translation fixes. The goal is to avoid replacing original game files when possible.

## Preferences Screen Override

Copy the complete `screen preferences():` block from the game into the patch file, stopping before the next non-indented block such as a `style` declaration. Add language buttons with Ren'Py's `Language()` action. If the label "Language" was not extracted, add it manually with `translate <language> strings:`.

## Label Override Pattern

```renpy
define config.label_overrides = {"start": "start_ch"}
```

When overriding a label:

- Copy the complete label.
- Rename the copied label.
- Map original to replacement with `config.label_overrides`.
- Update affected dialogue translation block identifiers by changing only the label-name prefix.
- If source edits are coordinated with the game source, a say-statement `id` clause can preserve an existing translation identifier after a minor source text change, and `label ... hide` can prevent a helper label from affecting generated translation identifiers.
- If execution reaches the label by fall-through instead of `call` or `jump`, override the previous label and add an explicit jump.

## Packaging Notes

Basic patch:

- Mirror the game's folder structure.
- Include modified `.rpy` and matching `.rpyc` when source files were changed.
- Include `game/tl/<language>/`.

Advanced patch:

- Keep as much as possible inside `game/tl/<language>/`.
- Include translated fonts/images in matching paths when needed.
- Test that removing the translation folder restores the original game behavior.
