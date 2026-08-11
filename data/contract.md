# 對話事件契約

> 分析層與決策層**只准讀這份契約定義的欄位**，不准碰原始資料欄位（← `prepare.md` CS-004）。
> 換資料源時只要重寫 loader 讓它產出符合本契約的表，上層一行不用改。

---

## 一列＝一個對話回合（turn），不是一則訊息

一則訊息可能被拆成多則推文連發。契約層先把**同一方連續發出、間隔小於 `TURN_GAP`
的訊息合併成一個 turn**，因為分析要問的是「來回幾次」，不是「打了幾則字」。

`TURN_GAP` 預設 60 秒，值與理由記在 `docs/ASSUMPTIONS.md`，不寫死在程式碼裡。

---

## 欄位

### 原始層（`confidence` 恆為 `observed`）

| 欄位 | 型別 | 說明 |
|------|------|------|
| `conversation_id` | string | 對話串識別碼。取該串**根訊息**的 `tweet_id` |
| `turn_index` | int | 該串內的回合序號，從 0 起 |
| `actor` | enum | `customer`／`company`。由原始 `inbound` 欄位直接映射 |
| `ts` | timestamp (UTC) | 該 turn 第一則訊息的時間 |
| `text` | string | 合併後的文字，訊息間以 `\n` 相接 |
| `msg_count` | int | 本 turn 合併了幾則原始訊息 |
| `source_ids` | string[] | 組成本 turn 的原始 `tweet_id`，供逐串核對 |
| `brand` | string | 企業端帳號識別碼。CS-007 橫向基準的分組欄位 |

### 推導層（每欄必須成組出現，缺一即視為未完成）

每個推導欄位 `X` 必須同時有 `X`、`X_rule_id`、`X_confidence` 三欄。

| 欄位 | 型別 | 推導的是什麼 | 首版預設信度 |
|------|------|------------|------------|
| `handoff_turn` | int \| null | 這串在第幾個 turn 從自動回覆交給真人 | `derived_weak` |
| `is_resolved` | bool \| null | 這串是否被解決 | `derived_weak` |
| `issue_category` | string \| null | 問題類別（供 M2 上游歸因分組） | `derived_weak` |
| `repeat_count` | int | 客戶重複描述同一問題的次數 | `derived_weak` |
| `wait_seconds` | int \| null | 客戶等待企業端回覆的秒數 | `observed`＊ |

> ＊`wait_seconds` 由兩個 `ts` 相減而得，算術而非推斷，故列 `observed`。
> 但它有一個已知失效情形（跨時區的營業時間），見 `docs/ASSUMPTIONS.md`。

### `confidence` 四態（封閉，不得擴充 ← `prepare.md` CS-005）

| 值 | 意義 |
|----|------|
| `observed` | 原始欄位直接讀出或純算術導出 |
| `derived_strong` | 推導規則，**且已在人工標註集上抽驗通過**（結果須寫進 `ASSUMPTIONS.md`） |
| `derived_weak` | 推導規則，未抽驗或抽驗失敗率高。**這是所有新規則的預設值** |
| `unknown` | 推不出來。此時欄位值為 `null`，不得填猜測值 |

---

## 範例

```json
{
  "conversation_id": "119237",
  "turn_index": 2,
  "actor": "company",
  "ts": "2017-10-31T22:10:47Z",
  "text": "We're sorry to hear that! Please DM us your order number and we'll take a look.",
  "msg_count": 1,
  "source_ids": ["119240"],
  "brand": "AmazonHelp",
  "handoff_turn": 2,
  "handoff_turn_rule_id": "R-002",
  "handoff_turn_confidence": "derived_weak",
  "is_resolved": null,
  "is_resolved_rule_id": null,
  "is_resolved_confidence": "unknown",
  "repeat_count": 1,
  "repeat_count_rule_id": "R-005",
  "repeat_count_confidence": "derived_weak",
  "wait_seconds": 1834,
  "wait_seconds_rule_id": null,
  "wait_seconds_confidence": "observed"
}
```

> 上例的 `text` 為示意用的典型句式，非資料集原文。真實文字一律不進文件
> （原始資料的再散布條款尚未查證，見 `docs/ASSUMPTIONS.md`）。

---

## 契約的三條硬規則

1. **推導欄位不得單獨出現**。少了 `_rule_id` 或 `_confidence` 的推導欄位，
   驗證腳本直接判定該列不合格。
2. **`unknown` 的值必須是 `null`**。不寫 0、不寫空字串、不寫「約」——
   0 會被下游當成真的數字算進平均。
3. **契約改動要有版本號**。改欄位定義就升 `contract_version` 並在 `CHANGELOG.md` 記一筆，
   否則舊產出與新產出混在一起、而且看不出來。
