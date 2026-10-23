# 辨識與解題的交接資料

分工或需要保留中間成果時，以 UTF-8 JSON 保存下列資訊；可以合併成一個檔案，或按題目拆檔。這是內容契約，不限定產生 HTML 的程式或視覺設計。

## 題目資料

- `id`：字串，包含題型與題號；有小題時也包含小題，例如 `single-14`、`multiple-24`、`calculation-2-b`。同一教材中唯一。
- `type`：`single_choice`、`multiple_choice`、`numeric` 或 `text`。
- `number`：原卷題號字串。
- `source`：物件，記錄圖片檔名、來源頁，以及必要時的裁切區域。
- `stem`：完整題幹字串，包含跨頁或共用條件。
- `options`：選項代號對文字的物件；非選擇題用空物件。
- `requiredSelections`：應選數量；非選擇題或原題未限定時用 `null`。
- `diagram`：物件或 `null`；描述必要圖形、座標、標記與數值，供重繪或裁圖核對。
- `uncertainties`：字串陣列，記錄仍無法確認的辨讀處；已核對無誤時為空陣列。

## 解題資料

- `id`：對應題目識別。
- `topic`、`insight`：主題及讀題切入點字串。
- `steps`：依序排列的推導字串陣列。
- `answerStatus`：`verified`、`ambiguous` 或 `unresolved`。
- `answer`：選擇題一律使用字母陣列，單選也是陣列；未能確定時用 `null`。若只有最近選項，保留 `answer: null`，在 `notes` 說明其差異。
- `answerText`：所有題型均可顯示的結論字串；數值題包含單位與精確／近似值，疑義題列出可確定的計算結果。
- `optionAnalysis`：選項代號對解析的物件，非選擇題為空物件。
- `pitfalls`、`notes`：字串陣列，分別為教學注意事項與題面疑義／適用假設。無內容時為空陣列。
- `practice`：包含 `type`、`stem`、`options`、`requiredSelections`、`answer`、`answerText` 與 `steps`；非選擇題另有 `grading`，明確定義數值容許誤差與單位，或可接受的文字答案。練習需有可確定的正解。

交接時核對：題目清單完整、識別對得上、資料型別一致、選項答案存在於該題選項中、多選答案數量符合題面，以及練習題有自己的題型。製作 agent 必須保留辨識及解題階段已列出的疑義；發現內容問題時回交核對，避免為了介面方便自行改答案。

將資料內嵌至 HTML 時安全處理文字及 script 結束標記，避免題幹中的特殊字元破壞文件或被解讀成可執行程式。可見文字優先以 `textContent` 或適當跳脫後的 HTML 建立。
