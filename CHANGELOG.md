# Changelog

本檔格式遵循 [Keep a Changelog](https://keepachangelog.com/)，版本規則遵循 [Semantic Versioning](https://semver.org/)。

## [Unreleased]

### Added
- `docs/OUTPUTS.md` — 各里程碑的產出形狀（M1～M5 表格規格）、判讀四關卡、
  建議強度三級（應該改／值得查／原因未定）、「沒有可改的東西」也是合格產出（← `prepare.md` CS-009、CS-010）
- `docs/METRICS.md` — 統計判斷九條原則，每條固定「原則 → 為什麼 → 在本專案落在哪」，
  末尾附原則與四道關卡的對應表（← `prepare.md` CS-009、CS-010）
- `prepare.md` CS-009、CS-010

### Changed
- `docs/ARCHITECTURE.md` — 目錄結構補兩份新文件；「里程碑與出口條件」節加註產出形狀不在本檔
- `README.md` — 專案結構與「快速開始」的閱讀順序由四份補為六份

> **M1 待辦（尚未開始，不屬於已完成的變更）**：`scripts/` 取得腳本與 `validate.py` 契約檢查、
> `analysis/` 基準線指標、`data/annotations/` 30 串人工標註集。
> 完成時移入上方 `Added`——**待辦與已完成不混在同一個標題下**，混了就看不出這版到底做了什麼。

## [0.1.0] - 2026-08-11

### Added
- 專案立項：`README.md`、`CLAUDE.md`、`prepare.md`（CS-000～CS-008）、`.gitignore`
- `data/contract.md` — 對話事件契約，含四態信度與三條硬規則
- `docs/ARCHITECTURE.md` — 四層職責、資料流、目錄結構、技術棧、M1～M5 里程碑與出口條件
- `docs/ASSUMPTIONS.md` — 推導規則 R-001～R-006、參數表、待查證五項、資料本身的四個限制

### Notes
- 本版**不含任何實作程式碼**，僅有文件與資料契約
- 所有推導規則狀態均為「待驗」（對應信度 `derived_weak`）——一行程式都還沒跑過
- 首版標 v0.1.0 而非 v1.0.0，理由見 `prepare.md` CS-008
- 資料來源的授權條款尚未查證，原始資料一律不進 repo（`docs/ASSUMPTIONS.md` §3-1）
