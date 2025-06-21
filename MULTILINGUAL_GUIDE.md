---
layout: page
title: "多語言博客文章指南 / Multilingual Blog Post Guide"
---

# 多語言博客文章指南 / Multilingual Blog Post Guide

## 概述 / Overview

這個系統允許你為同一篇文章創建多個語言版本，並在文章頂部顯示語言切換器。

This system allows you to create multiple language versions of the same article with a language switcher at the top of each post.

## 如何使用 / How to Use

### 1. 創建多語言文章 / Creating Multilingual Posts

為每種語言創建單獨的文章文件：
Create separate post files for each language:

```
_posts/2024-09-18-article-title.md      # 中文版 / Chinese version
_posts/2024-09-18-article-title-en.md   # 英文版 / English version
```

### 2. Front Matter 設置 / Front Matter Configuration

在每個文章的 front matter 中添加以下字段：
Add these fields to each post's front matter:

```yaml
---
layout: post
title: "Your Title"
# ... other fields ...
lang: zh # 語言代碼 / Language code (zh, en, etc.)
lang_ref: article-reference # 相同的參考ID / Same reference ID for all versions
---
```

### 3. 添加語言切換器 / Adding Language Switcher

在文章內容開始處添加：
Add at the beginning of your post content:

```liquid
{% include language_switcher.liquid %}
```

### 4. 支持的語言代碼 / Supported Language Codes

- `zh`: 中文 / Chinese
- `en`: English / 英文
- 可以添加更多語言 / More languages can be added

## 示例 / Example

### 中文版本 / Chinese Version

```yaml
---
layout: post
title: "我的文章標題"
lang: zh
lang_ref: my-article
---

{% include language_switcher.liquid %}

文章內容...
```

### 英文版本 / English Version

```yaml
---
layout: post
title: "My Article Title"
lang: en
lang_ref: my-article
---

{% include language_switcher.liquid %}

Article content...
```

## 自定義樣式 / Custom Styling

語言切換器的樣式可以在 `_includes/language_switcher.liquid` 中修改。
The language switcher styling can be modified in `_includes/language_switcher.liquid`.

## 添加新語言 / Adding New Languages

1. 在 `_includes/language_switcher.liquid` 中添加新的語言判斷
2. 使用對應的語言代碼創建新文章
3. 確保 `lang_ref` 在所有語言版本中保持一致

4. Add new language detection in `_includes/language_switcher.liquid`
5. Create new posts with corresponding language codes
6. Ensure `lang_ref` remains consistent across all language versions

## 注意事項 / Notes

- 所有同一文章的不同語言版本必須使用相同的 `lang_ref`
- 語言切換器只在有多個語言版本時才會顯示
- 建議保持文章的發布日期一致

- All language versions of the same article must use the same `lang_ref`
- Language switcher only appears when multiple language versions exist
- Recommended to keep publication dates consistent across versions

## 文件結構 / File Structure

```
├── _includes/
│   └── language_switcher.liquid    # 語言切換器組件
├── _posts/
│   ├── 2024-09-18-raptor-rag-tutorial.md     # 中文版文章
│   └── 2024-09-18-raptor-rag-tutorial-en.md  # 英文版文章
└── MULTILINGUAL_GUIDE.md          # 本指南文件
```
