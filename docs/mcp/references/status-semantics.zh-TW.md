# 狀態語義

## 用途

本頁說明外部接入方與 AI 編程助手應如何解釋目前公開的 payment 與 payout 狀態語義。

## Payment 狀態語義

目前 payment query 在公開回應中返回的是規範化字串狀態，典型包括：

- `PENDING`
- `SUCCESS`
- `CANCEL`
- `RISK`

下游系統應以這些公開返回值作為消費對象。

## Payout 狀態語義

目前 payout query 在公開回應中返回的是原始 `int` 狀態值。

目前映射示例包括：

- `0`：`PENDING_BUSINESS_APPROVAL`
- `1`、`2`、`3`、`6`、`7`、`8`、`9`：pending 家族狀態
- `4`：`CANCEL`
- `5`：`SUCCESS`

## 消費規則

- 不要假設 payment 與 payout 的狀態在線路格式上完全一致
- 應持久化平台返回的原始狀態值或原始狀態字串
- payout 的 `int` 值應透過目前公開狀態表做映射
- 對未來新增未知狀態，應保留日誌與安全處理邏輯

## 常見錯誤

- 把 payout 的 `status` 當成已經規範化的字串
- 把所有非成功狀態都壓扁成同一個失敗桶
- 本地歸一化後，丟掉平台原始狀態值
- 假設 payment 與 payout 狀態語義完全一樣

## 精確來源參考

- [`../../skill/references/crypto/statuses.zh-TW.md`](../../skill/references/crypto/statuses.zh-TW.md)
