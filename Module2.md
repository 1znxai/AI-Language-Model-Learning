利用筆電及開源llama.cpp 
練習大語言模型的訓練及量化
Llama-3-Taiwan-8B-Instruct (BF16)  4位元量化 



By Albert Tsai

研究動機
在自主學習1中，已經練習使用應用軟體(AnythingLLM)架設本地端大語言AI 模型，從相關學習資料中得知，可以用開源(llama.cpp)的方式自已下載AI模型再量化以縮小模型的大小；據了解，這些AI應用軟體也有參考或使用llama.cpp，所以也想要嘗試一下。

實作過程
建立Python虛擬環境
從 HuggingFace 下載 Model
使用 llama.cpp 來進行轉檔
使用 llama.cpp 來進行量化模型

建立Python虛擬環境 (參考資料5.2/5/3)

https://repo.anaconda.com/archive/下載及安裝Anaconda 
選擇適合的版本: Anaconda-2024.10-1-arm64.pkg

$conda create -n aienv-3 python=3.11在終端機建立一個Python(3.11)虛擬環境(aienv-3)
$ create acttivate aienv-3啟用 aienv-3

註:安裝虛擬環境是因為不同的應用會使用不同版本的套件，如此便可更方便管理且不會互相影響。

從 HuggingFace 下載 Model

Hugging Face是一個針對人工智慧的開源社群平台，使用者可以在此發表和共享預訓練模型、資料集和展示檔案等，等下會由此網站選擇一個開源LLM模型來實驗。
$  pip install -U "huggingface_hub[cli]"安裝Hugging Face下載程式

搜尋後決定下載開源 Llama-3-Taiwan-8B-Instruct
$  huggingface-cli download yentinglin/Llama-3-Taiwan-8B-Instruct

<img width="249" height="230" alt="image" src="https://github.com/user-attachments/assets/a8c5fc8e-ad8a-4106-8bbb-6176d16b6ff9" />









	…等待數十分鐘後下載完成，過程中曾有一次下載失敗，需要重執行下載命令。此模型目前大小為16.07 GB

使用 llama.cpp 來進行轉檔

llama.cpp 是 GGML 主要作者基於 Meta 最早 Open Souce LLM LLaMA 的 C/C++ 版本，目的是希望用 CPU 就可以 inference LLM；在社群眾人不懈的努力和貢獻下現今支援大多主流模型；而且可以對模型進行量化以減少大小
$ brew install cmake 
$ git clone https://github.com/ggerganov/llama.cpp.git
$ cd llama.cpp 
$ cmake -B build
$ cmake --build build --config Release在Mac下載及安裝llama.cpp 

安裝llama.cpp 的相關所需套件
$ pip install -r requirements.txt下載後先切換到llama目錄，再執行命令，安裝文件requirements.txt 指定的所需套件

使用 llama.cpp 來進行量化 
Model 轉換成 GGUF 格式 
註: GGUF (GPT-Generated Unified Format) 是一種儲存模型權重的檔案格式，交給 GGML 基底的程式去推理/預測

$cd ./models/
$mkdir Llam-3-Taiwan-8B-Instruct
$mv /Users/albert/.cache/huggingface/hub/models—yenting--Llam-3-Taiwan-8B-Instruct snapshots/f4bxx.../*   ./Llam-3-Taiwan-8B-Instruct先將Llama-3-Taiwan-8B-Instruct 移到llama.cpp 目錄

註: f4bxx… 根據下載時間及版本會不一樣

$ python ../convert_hf_to_gguf.py Llama-3-Taiwan-8B-Instruct --outtype f16 --outfile Llam-3-Taiwan-8B-Instruct.gguf	再將模型轉成GGUF



執行量化 (Q4_K_M)
$ ../build/bin/llama-quantize Llama-3-Taiwan-8B-Instruct.gguf Llama-3-Taiwan-8B-Instruct-Q4-K-M.gguf Q4_K_M目前還在目錄 ./models，執行以下命令進行量化，等待一段時間完成。

註:由網頁介紹可知，如果追求較低成本和保持模型效能的情況推薦使用用 Q5_K_M，如果想更節省 RAM，則可以考慮 Q4_K_M。一般來說，帶有 K_M 的版本會比 K_S 的版本表現更佳。不過不建議使用 Q2_K 或 Q3_* 等版本，因為它們會顯著降低模型的整體性能。

<img width="870" height="362" alt="image" src="https://github.com/user-attachments/assets/637abb20-cb1e-44b4-92ce-e2840c09aa21" />

量化示意圖（Source）





成果展現
量化模型的大小
Llama-3-Taiwan-8B-Instruct-Q4-K-M.gguf 大小為4.93GB; 為16 位元版本的 4.93/16.07 大約3成

$ ../build/bin/llama-server -m Llama-3-Taiwan-8B-Instruct-Q4-K-M.gguf 架設llama.cpp Server (也可直接以命令列執行)

通過 HTTP API 來存取模型，預設會在 http://127.0.0.1:8080/ 開啟一個 LLM Service，參數的部分也可以進到 server 再調整。如同chatGPT.com的使用，在Say something欄位下可以輸入中英的要求或問題，測試看看自已架設及量化的AI 大語言模型。

<img width="782" height="1396" alt="image" src="https://github.com/user-attachments/assets/79920502-4304-45f8-bd82-b6f19c21afa3" />

反思與收穫
這個部分雖然一樣是在本地端的筆電架設大語言模型，但是跟上一個作品不一樣的地方是，進一步試著去下載大語言模型之後，自己用開源的程式去進行量化以減少模型的大小。在架設的過程中，步驟看似簡單，但每個步驟的程式碼都需要一再檢視，像有時候名稱不對就無法繼續下一步。或是需要pwd確認一下自己位於哪個目錄，否則程式碼也無法在不對的位子運行。在AI日新月異的時代，也要持續向前學習。是之後可探討的事情。

參考資料
將 HuggingFace 模型轉換為 GGUF 及使用 llama.cpp 進行量化
Python 環境安裝設定 – Anaconda
利用conda建立及管理Python虛擬環境 
https://github.com/ggerganov/llama.cpp
https://github.com/ggerganov/llama.cpp/doc/build.md
