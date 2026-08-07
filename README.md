# 嘉立创 EDA 专业版 API 多语言翻译

本仓库存储嘉立创 EDA / EasyEDA 专业版扩展 API 类型定义的翻译数据。

> 翻译源文件由流水线自动生成和维护，
> 本仓库 **只负责翻译文本**，不修改元数据（`_meta`）和原文对照（`_source`）。

## 如何参与翻译

### 1. Fork 本仓库并创建分支

### 2. 找到需要翻译的文件

翻译文件按语言代码放在对应目录下：

```
en/   ← 英文翻译
ja/   ← 日文翻译
```

每个 `.json` 文件对应一个 API 声明（类、方法、接口、枚举、类型别名等）。文件名格式为 `{ClassName}.json` 或 `{ClassName}.{methodName}.json`。

> 如果你不清楚某个文件对应哪个 API，可以在 [嘉立创 EDA 专业版 API 文档](https://prodocs.lceda.cn/cn/api/reference/pro-api.html) 中搜索 `_meta.id` 的值。

### 3. 编辑 JSON 文件

打开 JSON 文件，对照 `_source`（简体中文原文），将 `null` 替换为目标语言翻译。

**示例** — `en/DMT_SelectControl.getCurrentDocumentInfo.json`：

```jsonc
{
  // ⬇ 元数据 — 不要修改
  "_meta": {
    "id": "DMT_SelectControl.getCurrentDocumentInfo",
    "kind": "method",
    "name": "getCurrentDocumentInfo",
    "className": "DMT_SelectControl",
    "sourceFile": "dist/api-types.d.ts"
  },

  // ⬇ 简体中文原文对照 — 不要修改
  "_source": {
    "title": "获取当前文档的属性",
    "remarks.text.0": "将会获取当前打开且拥有最后输入焦点的文档的文档类型、UUID、所属工程的 UUID 或所属库的 UUID",
    "returns": "文档类型、UUID、所属工程的 UUID、所属库的 UUID 组成的对象，如若为 `undefined` 则获取失败"
  },

  // ⬇ 翻译字段 — 在这里填入翻译
  "title": "Get current document info",          // ✅ 已翻译
  "remarks.text.0": null,                        // ⬅ 待翻译，将 null 替换为译文
  "returns": "Returns an object containing..."   // ✅ 已翻译
}
```

### 翻译规则

| 字段类型 | 说明 |
|---------|------|
| `title` | 标题/摘要（类、方法、接口等的描述） |
| `remarks.text.{i}` | `@remarks` 中的文本段落（代码块不在此列） |
| `params.{paramName}` | `@param` 参数说明 |
| `returns` | `@returns` 返回值说明 |
| `deprecated` | `@deprecated` 弃用说明 |
| `members.{NAME}` | 枚举成员注释 |
| `properties.{name}` | 接口/类属性注释 |
| `example.text.{i}` | `@example` 中的文本段落（代码块不在此列） |

### 注意事项

- **只修改值为 `null` 的字段**，已有翻译不要随意改动（除非有误）
- **`_meta` 和 `_source` 不要修改**，它们会在下次同步时被覆盖
- **保留反引号** `` ` `` 和 Markdown 格式
- **代码块、版本标记**（如 `ADD since EDA v4.2`）不会出现在翻译字段中，无需处理
- 如果某个 `null` 字段不打算翻译，**保持 `null`** 即可，生成时会自动回退原文

### 4. 提交 Pull Request

翻译完成后提交 PR 到本仓库。review 通过后，pro-api-types 项目下次构建时会自动应用你的翻译。

## 字段 Key 速查

| 元素 | key |
|------|-----|
| 标题 | `title` |
| @remarks 文本段 | `remarks.text.0`, `remarks.text.1`... |
| @param foo | `params.foo` |
| @returns | `returns` |
| @deprecated | `deprecated` |
| 枚举成员 FOO | `members.FOO` |
| 属性 bar | `properties.bar` |
| @example 文本段 | `example.text.0`, `example.text.1`... |

## 批量查找待翻译条目

```bash
# 查找英文翻译中所有 null 字段
grep -r ': null' en/ | wc -l

# 查找特定文件中的待翻译 key
python3 -c "
import json
d = json.load(open('en/YourFile.json'))
for k, v in d.items():
    if not k.startswith('_') and v is None:
        print(f'{k}  ← 原文: {d[\"_source\"].get(k, \"\")}')
"
```
