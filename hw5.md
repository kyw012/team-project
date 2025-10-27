# 小組作業 5

## 一、UML 類別圖（Class Diagram）

系統主要包含四個核心類別：使用者（User）、動作辨識（ActionRecognition）、報告生成（ReportGenerator）、與健身建議（FitnessAdvisor）。

```mermaid
classDiagram
    class User {
        +userID: int
        +name: string
        +email: string
        +login()
        +uploadVideo()
    }

    class ActionRecognition {
        +detectPose(video)
        +calculateAccuracy()
        +displayScore()
    }

    class ReportGenerator {
        +analyzeData()
        +generateChart()
        +createReport()
    }

    class FitnessAdvisor {
        +sendRequest(data)
        +getSuggestion()
    }

    User --> ActionRecognition : 上傳影像
    ActionRecognition --> ReportGenerator : 傳送辨識結果
    ReportGenerator --> FitnessAdvisor : 提供分析數據
    FitnessAdvisor --> User : 回傳建議
```

## 二、循序圖（Sequence Diagrams）

### 案例一：登入系統

```mermaid
sequenceDiagram
    participant U as 使用者
    participant S as 系統
    participant DB as 資料庫

    U->>S: 輸入帳號與密碼
    S->>DB: 驗證帳密
    DB-->>S: 回傳驗證結果
    S-->>U: 顯示登入成功或錯誤訊息
```

### 案例二：上傳與辨識動作

```mermaid
sequenceDiagram
    participant U as 使用者
    participant RG as 報告生成模組
    participant FA as 健身建議模組
    participant API as ChatGPT/Gemini API

    U->>RG: 請求健身建議
    RG->>FA: 傳送使用者運動數據
    FA->>API: 發送請求
    API-->>FA: 回傳健身建議
    FA-->>U: 顯示個人化建議

```

### 案例三：獲取個人化健身建議

```mermaid
sequenceDiagram
    participant U as 使用者
    participant RG as 報告生成模組
    participant FA as 健身建議模組
    participant API as ChatGPT/Gemini API

    U->>RG: 請求健身建議
    RG->>FA: 傳送使用者運動數據
    FA->>API: 發送請求
    API-->>FA: 回傳健身建議
    FA-->>U: 顯示個人化建議
```


## 三、活動圖（Activity Diagrams）

### 案例一：登入系統

```mermaid
flowchart TD
    A[開始] --> B[輸入帳號與密碼]
    B --> C{帳密正確？}
    C -->|是| D[登入成功]
    C -->|否| E[顯示錯誤訊息]
    D --> F[進入主畫面]
    E --> F
    F --> G[結束]
```

### 案例二：上傳與辨識動作

```mermaid
flowchart TD
    A[開始] --> B[上傳影片]
    B --> C[系統進行動作辨識]
    C --> D{動作正確？}
    D -->|是| E[顯示高分與鼓勵訊息]
    D -->|否| F[顯示教學影片]
    E --> G[更新分析資料]
    F --> G
    G --> H[結束]
```
### 案例三：獲取個人化健身建議


```mermaid
flowchart TD
    %% 範例：更詳細、更真實的活動圖
    A[🏁 開始] --> B[使用者登入系統]
    B --> C[選擇「獲取健身建議」功能]
    C --> D[系統擷取最新運動紀錄與分析報告]
    D --> E{分析資料是否完整？}
    E -->|否| F[提示使用者補充運動資料]
    F --> B
    E -->|是| G[傳送分析結果至 ChatGPT/Gemini API]
    G --> H[等待 API 回覆]
    H --> I{回覆成功？}
    I -->|否| J[顯示錯誤訊息並允許重新請求]
    J --> G
    I -->|是| K[解析回覆內容並生成建議報告]
    K --> L[顯示個人化健身建議]
    L --> M[允許使用者評價或儲存建議]
    M --> N[結束]
```
