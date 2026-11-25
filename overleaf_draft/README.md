# Overleaf IEEE Conference Draft (`overleaf_draft/`)

本資料夾為由 `物聯網_Final_Project_Proposal.pdf` 轉換生成之 IEEE Conference LaTeX 草稿（`\documentclass[conference]{IEEEtran}`，支援繁體中文 `xeCJK`）。

## 檔案結構

- `main.tex`：IEEE 雙欄論文主檔（含摘要、緒論、相關研究與表、TikZ 系統架構圖、資料策略與模型、Algorithm 1 虛擬碼、評估設計）
- `references.bib`：完整 12 篇參考文獻之 BibTeX 格式
- `latexmkrc`：設定 `$pdf_mode = 5;`，讓 Overleaf 自動以 **XeLaTeX** 編譯

## 上傳至 Overleaf 步驟

1. 直接上傳上層目錄已打包好的 `overleaf_ieee_draft.zip` 至 Overleaf（New Project $\rightarrow$ Upload Project）。
2. Overleaf 會自動讀取 `latexmkrc` 並啟用 `XeLaTeX` 編譯 `main.tex`（若手動確認，可在左上角 **Menu $\rightarrow$ Compiler** 確認為 **XeLaTeX**）。
3. 編譯後請於 `main.tex` 第 45–53 行替換 `[作者姓名]`、`[系所名稱]`、`[學校或機構名稱]` 與 Email。
