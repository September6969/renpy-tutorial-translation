# Ren'Py 教程翻译技能 (Ren'Py Tutorial Translation Skill)

这是一个用于根据附带的中文教程处理 Ren'Py 游戏翻译的 Codex 技能：

- [Renpy游戏翻译教程V2.0.docx](references/Renpy游戏翻译教程V2.0.docx)
- [精简版教程笔记](references/tutorial-notes.md)

## 涵盖内容

- 生成和更新 Ren'Py 翻译文件。
- 处理 `translate <language> ...` 对话块。
- 处理 `translate <language> strings`、`old` 和 `new` 字符串。
- 查找 Ren'Py 可能无法自动提取的字符串。
- 添加语言切换功能。
- 修复常见的翻译问题，如文本溢出、字符缺失、图片文字、插值以及拼接字符串。
- 创建基础和高级翻译补丁，包括 `zzz.rpy`、`init offset` 和 `label` 覆盖。

## 安装

将此文件夹克隆或复制到您的 Codex 技能目录中：

```powershell
git clone https://github.com/September6969/renpy-tutorial-translation.git "$env:USERPROFILE\.codex\skills\renpy-tutorial-translation"
```

然后启动一个新的 Codex 会话以便识别该技能。

## 使用方法

在执行 Ren'Py 翻译任务时，要求 Codex 使用 `$renpy-tutorial-translation`。

示例：

```text
使用 $renpy-tutorial-translation 根据教程检查此 Ren'Py 翻译补丁。
```

## 文件内容

```text
SKILL.md
agents/openai.yaml
references/tutorial-notes.md
references/Renpy游戏翻译教程V2.0.docx
```
