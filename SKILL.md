---
name: renpy-tutorial-translation
description: Follow the specific local tutorial "Renpy游戏翻译教程V2.0.docx" for Ren'Py game translation work. Use when Codex is asked to translate, migrate, repair, test, package, or make a language patch for a Ren'Py game according to that tutorial, especially tasks involving game/tl/<language> files, old/new translation strings, translate blocks, language switching, fonts/styles, extracted strings, advanced zzz.rpy patches, or label overrides.
---

# Ren'Py Tutorial Translation

Use this skill as an execution checklist for the local tutorial, not as a project-specific style guide. The source tutorial is `D:/download/Renpy游戏翻译教程V2.0.docx`; read it directly when exact wording or examples matter. For a compact index, see [tutorial-notes.md](references/tutorial-notes.md).

## Operating Rules

- Treat `game/tl/<language>/` as the primary work area for translation.
- Avoid editing original source scripts when the tutorial provides a translation-file or advanced-patch solution.
- Preserve Ren'Py syntax exactly: indentation with spaces, balanced quotes, text tags, interpolation, and `old` strings.
- Do not delete original `.rpyc` files; the tutorial warns this can break AST-based translation and old saves.
- When source edits are unavoidable, prefer putting overrides in a translation-folder patch file as described in the advanced patch section.
- Validate with the game's Ren'Py executable or SDK after changes. Check `errors.txt`, `traceback.txt`, and `text_overflow.txt` when behavior or layout is wrong.

## Workflow

1. Confirm the project structure.
   - Locate the game root, `game/`, `game/tl/`, and the target language folder.
   - If `.rpy` files are missing, the tutorial's route is to unpack `.rpa` or decompile `.rpyc` before generating translations.

2. Work from generated translation files.
   - Generate translation files with Ren'Py using a stable language name.
   - Remember Ren'Py is case-sensitive; the language folder name must match the language identifier used in `translate <language>`.
   - For existing translations or updates, copy/migrate prior translation files carefully, then compare against newly generated files.
   - For game updates, copy the old `game/tl/<language>/` folder into the new game first, then generate translations with the same language identifier and without replacing existing translations so Ren'Py appends only newly untranslated strings.
   - To translate from an existing third-language translation, copy that `game/tl/<other-language>/` folder to a new `game/tl/<language>/` folder and replace translation headers from `translate <other-language>` to `translate <language>`.

3. Translate the two Ren'Py translation forms correctly.
   - Dialogue blocks use `translate <language> <label>_<hash>:`. Fill only the translated dialogue line; leave surrounding Ren'Py code intact.
   - Menu/UI/variable strings use `translate <language> strings:` with `old` and `new`. Never change `old`; write the translation only in `new`.
   - Identical `old` strings share one translation globally, so check context when a short word has multiple meanings.

4. Find strings Ren'Py may not extract.
   - Search original scripts for strings in `Character("...")`, `text "..."`, `textbutton "..."`, `tooltip "..."`, `renpy.input("...")`, `renpy.notify(...)`, and text-valued `default`, `define`, or `$ variable = "..."`.
   - Either wrap source strings with `_()` before regenerating translations, or manually add matching `old`/`new` entries in a translation file.
   - Copy the full string including text tags and interpolation when manually adding old/new entries.
   - For repeated short strings with different meanings, do not rely on a single global old/new translation; use a more specific source extraction, a source-side `_()` marker, or an advanced patch when context requires different translations.

5. Add language switching through the tutorial's screen method.
   - Copy the relevant preferences screen into a patch file under `game/tl/<language>/`.
   - Add language buttons with `Language(None)` and `Language("<language>")` as appropriate.
   - If "Language" was not extracted, add a manual `translate <language> strings:` entry for it. Avoid duplicate old/new entries.

6. Use advanced patches for source-level changes.
   - Create a uniquely named `.rpy` file in `game/tl/<language>/`, commonly something late-loading such as `zzz.rpy`.
   - Add `init offset = 1` before screen/style/define overrides so they load after the original definitions.
   - Put default language, UI style fixes, replacement functions, and screen overrides here when possible.
   - Use `define config.default_language = "<language>"` to set the first-launch default language. Use `config.language` only when the intent is to force a language every run.
   - Use `preferences.language` checks to limit risky visual or text replacement changes to the target language.

7. Use label overrides only when needed.
   - Copy the complete original label into the patch file, rename it, and map it with `define config.label_overrides = {"original": "replacement"}`.
   - Update affected translated dialogue block identifiers by replacing only the label-name part, not the hash.
   - Remember old/new string translations are label-independent, but dialogue block translations include the label name.
   - If the original flow reaches a label by natural fall-through rather than `call` or `jump`, the tutorial notes that overriding the previous label and adding an explicit `jump` may be required.

8. Package according to patch type.
   - A basic patch mirrors the original folder structure and may include modified `.rpy` and matching `.rpyc`.
   - An advanced patch should keep changes inside `game/tl/<language>/` where possible so removing the language folder restores the original game.
   - Include translated images and fonts under mirrored paths when they are part of the translation.

## Common Fix Areas

- Startup or runtime errors: inspect `errors.txt` for generation/startup failures and `traceback.txt` for runtime failures. Common translation-caused causes include broken quotes, tabs/indentation, unclosed text tags, missing braces, and changed special symbols such as `%`, `\`, or `/`.
- Text overflow: shorten wording, split one source line into multiple translated lines in the dialogue block, or adjust text size/style/textbox width when wording alone is insufficient.
- Missing glyphs or squares: add or override a font/style that supports the target language. Prefer language-folder patch overrides when the change should not affect other languages.
- Image text: translate image assets when text is baked into images. Preserve the original path structure in the patch.
- Identical strings with different meanings: Ren'Py old/new string translation is global by exact `old` value, so inspect context and use source extraction or overrides when one translation cannot fit all uses.
- Variables and interpolation: translate the variable source if it is a translatable string. For ordinary variables whose values must be translated at display time, use translation-aware interpolation such as `[variable!t]`; role/name variables may behave differently depending on how `Character(...)` is defined.
- Grammar-dependent translations: dialogue translations can contain Ren'Py `if`/`else` code when a variable value changes the correct translated sentence. Preserve indentation and syntax exactly.
- Concatenated strings: strings built with Python concatenation may not be extractable as complete old/new strings. The tutorial suggests `__()` for translatable parts, `[variable!t]` where appropriate, and patch/label override techniques when source changes must be isolated.
- Last-resort replacement: `config.replace_text` can replace display text immediately before rendering, but it can also affect already translated text. Restrict it narrowly and combine it with a target-language check when possible.

## Validation

- Run Ren'Py lint/compile when available.
- Start the game and test at least menus, preferences/language switching, saves, representative dialogue, choices, text overflow, fonts, and any overridden labels/screens.
- If the game fails to launch, inspect `traceback.txt` and `errors.txt` first; if text spills out, inspect `text_overflow.txt`.
