# course-workflows

中國文化大學 — 楊老師課程共用的 GitHub Actions 可重複使用 workflow。

**這個 repo 必須是 public**，才能被各課程 org 底下的 private 作業 repo 引用。

## 可用 workflow

| 檔案 | 用途 |
|---|---|
| `.github/workflows/cpp-grade.yml` | C++ 編譯 + 測資比對自動批改 |

## 在作業 repo 中引用

```yaml
name: 自動批改
on:
  push:
  pull_request:
  workflow_dispatch:

jobs:
  grade:
    uses: yungder/course-workflows/.github/workflows/cpp-grade.yml@main
    with:
      std: "c++17"
```

## 可調參數（cpp-grade.yml）

| 參數 | 預設 | 說明 |
|---|---|---|
| `std` | `c++17` | C++ 標準，可改 `c++20` |
| `src-glob` | `src/*.cpp` | 原始碼位置 |
| `cases-dir` | `tests/cases` | 測資資料夾 |
| `timeout-sec` | `5` | 單筆測資秒數上限 |

## 改版注意

各課程 repo 是用 `@main` 引用，**改動這裡會立刻影響所有進行中的作業**。
學期中若要改批改邏輯，建議先開 tag（例如 `v115-1`），把作業模板改成 `@v115-1` 再動 main。
