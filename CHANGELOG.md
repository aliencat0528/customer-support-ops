# Changelog

本檔格式遵循 [Keep a Changelog](https://keepachangelog.com/)，版本規則遵循 [Semantic Versioning](https://semver.org/)。

## [Unreleased]

### Added
- （M1）`scripts/` 取得腳本與 `validate.py` 契約檢查
- （M1）`analysis/` 基準線指標
- （M1）`data/annotations/` 30 串人工標註集

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
