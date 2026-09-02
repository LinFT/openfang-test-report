# openfang-test-report

在 Google Colab 免費環境上評估 OpenFang（RightNow-AI 的 Rust 單一 binary agent OS）的實測紀錄。目標不是跑分，而是回答一個問題：**把它接上公網前台（例如 LINE）之前，哪些宣稱成立、哪些預設會咬人。**

## 方法

一本逐格可跑的 Colab 筆記本，用免費 API 層（Mistral 主力，NVIDIA、Gemini 多桶備援）重建環境，對 agent 做越權、記憶、故障切換三類測試，再以零額度的五層審計（宣告、快照、注入、切換、共用記憶）交叉驗證 log、SQLite 與 API 各自說的話。所有結論都附可重跑的格與判讀規則，能被推翻的就標「待補」。

## 核心結論

核心機制符合宣稱：manifest 工具白名單有注入與執行兩層保障；核准閘門攔得住模型幻覺出的工具；`[[fallback_providers]]` 對任何錯誤都會切換且 agent 無感。

真正的風險在記憶子系統的「單一使用者」預設：`memory_store` 是全 agent 共用的 key-value；`user_name` 會被 kernel 推送進每一個 agent 的 prompt；沒有嵌入模型時 episodic 記憶等於關閉且不警告。三者都不是 bug，接公網卻必須在架構上避開。

## 閱讀順序

`OPENFANG_HANDOFF_v2.md` 是規則、審計方法與延伸題的定稿；`openfang_colab_v2_x` 是對應的筆記本，取最高版號。v1 檔案保留作考古，其中被修正的結論已在 v2 標明。
