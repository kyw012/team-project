# 系統實體關係圖 (ERD)

```mermaid
erDiagram
    %% 實體定義
    User {
        int UserID PK "使用者ID"
        string Email "電子郵件/帳號"
        string Password "密碼 (Hash)"
    }

    Exercise {
        int ExerciseID PK "訓練項目ID"
        string Name "項目名稱 (深蹲, 平板撐)"
    }

    TrainingRecord {
        int RecordID PK "紀錄ID"
        int UserID FK "使用者ID"
        int ExerciseID FK "訓練項目ID"
        date Date "訓練日期"
        int Quantity "次數/時間 (15, 2, 20)"
        int AiScore "AI 評分 (75-92)"
        float DurationMinutes "持續時間(分鐘)"
        float CaloriesConsumed "消耗卡路里"
    }

    AI_Feedback {
        int FeedbackID PK "建議ID"
        int RecordID FK "訓練紀錄ID"
        string AiSuggestion "AI 教練的建議文字"
        string IssueDetected "偵測到的問題 (Valgus collapse)"
    }

    %% 組合實體 (Composite Entities) - 基於 TrainingRecord 聚合

    WeeklySummary {
        int UserID FK "使用者ID"
        float TotalTime "本週總訓練時間"
        int TotalCalories "本週總卡路里"
        int TotalReps "本週總訓練次數"
    }

    HistoryTrend {
        int UserID FK "使用者ID"
        date WeekEndDate "週結尾日期"
        float AverageScore "週平均分數"
        int WorkoutDays "本週運動天數"
    }

    %% 關係定義
    User ||--o{ TrainingRecord : 進行 (performs)
    Exercise ||--o{ TrainingRecord : 屬於 (is_of)
    TrainingRecord ||--o| AI_Feedback : 產生建議 (generates_feedback)

    %% 組合實體與原始實體的關係
    User ||--o{ WeeklySummary : 彙總 (aggregates)
    User ||--o{ HistoryTrend : 追蹤 (tracks)
```
##  資料庫結構分析

系統中主要涵蓋四個核心實體（Entity）：使用者、訓練紀錄、訓練項目、訓練建議。

| 實體名稱 (Entity) | 描述 | 識別碼 (PK) |
| :--- | :--- | :--- |
| **使用者 (User)** | 系統的使用者帳號資訊。 | UserID |
| **訓練項目 (Exercise)** | 系統中預設的運動項目，如深蹲、平板撐等。 | ExerciseID |
| **訓練紀錄 (TrainingRecord)** | 每次訓練的詳細歷史數據和結果。 | RecordID |
| **訓練建議 (AI_Feedback)** | AI 教練基於訓練紀錄給出的個性化建議。 | FeedbackID |


## 組合實體

1.  **本週彙總數據 (WeeklySummary)**：這是從多筆 **訓練紀錄** 聚合計算而來的數據（總訓練時間、總卡路里、總次數）。它依賴於 \`TrainingRecord\` 實體。
2.  **歷史趨勢數據 (HistoryTrend)**：這是從多筆 **訓練紀錄** 聚合計算而來的分數變化趨勢和運動天數統計。它也依賴於 \`TrainingRecord\` 實體。


##  關鍵實體與屬性說明

| 實體名稱 | 關係類型 | 關鍵屬性 |
| :--- | :--- | :--- |
| **User** | **1-對多 (1:M)** | \`UserID\` (PK), \`Email\` |
| **Exercise** | **1-對多 (1:M)** | \`ExerciseID\` (PK), \`Name\` (深蹲, 平板撐) |
| **TrainingRecord** | **多對多 (M:M)** 之間的中介實體 | \`RecordID\` (PK), \`AiScore\`, \`Quantity\`, \`CaloriesConsumed\` |
| **AI\_Feedback** | **1-對1 (1:1)** | \`FeedbackID\` (PK), \`AiSuggestion\`, \`IssueDetected\` (e.g., Valgus collapse) |
| **WeeklySummary** | **組合實體** | \`TotalTime\`, \`TotalCalories\`, \`TotalReps\` (來自 TrainingRecord 聚合) |
| **HistoryTrend** | **組合實體** | \`AverageScore\`, \`WorkoutDays\` (來自 TrainingRecord 聚合) |
