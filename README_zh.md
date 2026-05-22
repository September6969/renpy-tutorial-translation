# Ren'Py 教程翻译技能

这是一个用于处理 Ren'Py 游戏翻译的 Codex skill，依据仓库内附带的中文教程整理：

- [Renpy游戏翻译教程V2.0.docx](references/Renpy游戏翻译教程V2.0.docx)
- [教程要点索引](references/tutorial-notes.md)

## 覆盖内容

- 生成和更新 Ren'Py 翻译文件。
- 处理 `translate <language> ...` 对话块。
- 处理 `translate <language> strings`、`old` 和 `new` 字符串。
- 保持 `old` 精确匹配，检查菜单选项、tag、插值、状态后缀和 UTF-8 编码安全。
- 查找 Ren'Py 可能无法自动提取的字符串。
- 添加语言切换功能。
- 修复常见翻译问题，例如文本溢出、缺字/方块字、图片文字、插值和拼接字符串。
- 创建基础和进阶翻译补丁，包括 `zzz.rpy`、`init offset`、语言作用域的 `python/style` 覆盖、资源替换和 label 覆盖。

## 安装

把这个仓库克隆或复制到 Codex skills 目录：

```powershell
git clone https://github.com/September6969/renpy-tutorial-translation.git "$env:USERPROFILE\.codex\skills\renpy-tutorial-translation"
```

然后启动新的 Codex 会话，让 Codex 重新发现这个 skill。

## 使用

在处理 Ren'Py 翻译任务时，让 Codex 使用 `$renpy-tutorial-translation`。

示例：

```text
使用 $renpy-tutorial-translation 根据教程检查这个 Ren'Py 翻译补丁。
```

## 文件内容

```text
SKILL.md
agents/openai.yaml
references/tutorial-notes.md
references/Renpy游戏翻译教程V2.0.docx
```
