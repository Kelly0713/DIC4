# DIC4: Personalized Weather Assistant (OpenSpec + IEEE Overleaf Draft)

本專案源自 `物聯網_Final_Project_Proposal.pdf`，整合 **OpenSpec** 規格驅動文件與 **IEEE Conference Overleaf LaTeX 草稿**，用於管理「個人化天氣助理（Personalized Weather Assistant, PWA）」之系統規格與論文發表草稿。

## 專案結構

```text
DIC4/
├── openspec/
│   ├── specs/                                  # 主線系統規格目錄
│   └── changes/
│       └── personalized-weather-assistant/     # OpenSpec 變更提案、設計、分項規格與任務
│           ├── proposal.md
│           ├── design.md
│           ├── tasks.md
│           └── specs/
│               ├── itinerary-normalization/spec.md
│               ├── environmental-data-alignment/spec.md
│               ├── hybrid-decision-engine/spec.md
│               ├── preference-feedback-memory/spec.md
│               └── ieee-overleaf-draft/spec.md
├── overleaf_draft/                             # 可直接匯入 Overleaf 之 IEEEtran + xeCJK 論文專案
│   ├── main.tex
│   ├── references.bib
│   ├── latexmkrc
│   └── README.md
├── overleaf_ieee_draft.zip                     # Overleaf 一鍵上傳壓縮包
└── 物聯網_Final_Project_Proposal.pdf           # 原始專題企劃書 PDF
```

## 常用指令

### 1. 驗證 OpenSpec 規格
```powershell
openspec validate --all --strict
openspec status --change "personalized-weather-assistant"
```
> 若 Windows PowerShell 因執行原則無法執行 `openspec.ps1`，請改用 `openspec.cmd`。

### 2. 上傳 Overleaf
將 `overleaf_ieee_draft.zip` 上傳至 Overleaf（New Project → Upload Project），系統將透過 `latexmkrc` 自動以 `XeLaTeX` 編譯 `main.tex`。

### 3. 推送至 GitHub
```bash
git remote add origin <YOUR_GITHUB_REPO_URL>
git branch -M main
git push -u origin main
```
