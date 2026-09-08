# course-workflows

中國文化大學 — 楊老師課程共用的 GitHub Actions 可重複使用 workflow。

**這個 repo 必須是 public**，才能被各課程 org 底下的 private 作業 repo 引用。

## 可用 workflow

| 檔案 | 用途 |
|---|---|
| `.github/workflows/cpp-grade.yml` | C++ 多小題自動批改：`src/<題名>.cpp` 對應 `tests/<題名>/`，各題各自編譯、各自跑測資 |

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
| `src-dir` | `src` | 原始碼資料夾，內含各小題 `<題名>.cpp` |
| `tests-dir` | `tests` | 測資根資料夾，內含各小題同名子資料夾 `tests/<題名>/*.in` `*.out` |
| `timeout-sec` | `5` | 單筆測資秒數上限 |

### 多小題規則

`src-dir` 底下**每一支 `.cpp`** 都會被獨立編譯、獨立批改。
檔名 `ch2_1.cpp` 對應測資資料夾 `tests/ch2_1/`；沒有對應測資資料夾的題目
只檢查編譯過不過，不算分。一個小題編譯失敗**不影響**其他小題繼續批改，
最終結果是「全部小題都編譯成功且全部測資通過」才算整體綠燈。

## 改版注意

各課程 repo 是用 `@main` 引用，**改動這裡會立刻影響所有進行中的作業**。
學期中若要改批改邏輯，建議先開 tag（例如 `v115-1`），把作業模板改成 `@v115-1` 再動 main。
