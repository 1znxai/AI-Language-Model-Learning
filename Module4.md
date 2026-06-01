AI語音複製&TTS 
AI Voice Clone & Text To Speech





By Albert Tsai

研究動機
在前面的自主學習中已經練習了大語言模型，類神經網路還有深度學習，現在想要繼續練習看看能不能用AI來複製自已的聲音，把文字轉成語音(Text To Speech)。

實作過程
錄一段自已的聲音:60秒中英文的文章
存成.wav或 .m4a
詢問Grok，如何以Colab架設簡單的AI Clone 
Colab 執行開源tortoise tts遇到版本不相容問題
找尋其他方案:OpenVoice
用conda 安裝OpenVoice及解決過程中的問題
ffmpeg語言轉檔程式安裝
自然語言工具包(NLTK)及缺失的套件安裝
執行及得到結果 
詢問Grok，如何以Colab架設簡單的AI Clone
https://colab.research.google.com/drive/1wVVqUPqwiDBUVeWWOUNglpGhU3hg_cbR?usp=sharing
複製以上的tortoise-tts.ipynb到自己的雲端空間
執行後遇到版本不相容的問題
看到一大堆incompatible，很頭痛
以Conda虛擬環境安裝也無法解決
且tortoise只支援英文
找尋其他開源的專案

<img width="565" height="100" alt="image" src="https://github.com/user-attachments/assets/1b49c596-d3ad-4e1e-a06f-618bbc1776ef" />






救星: OpenVoice ()
https://github.com/myshell-ai/OpenVoice
安裝較簡單，版本相依性較少
且支援多國語言
利用我們之前的自主學習的conda架設環境

安裝說明:
https://github.com/myshell-ai/OpenVoice/blob/main/docs/USAGE.md

conda create -n openvoice python=3.9
conda activate openvoice
git clone https://github.com/myshell-ai/OpenVoice.git
cd OpenVoice
pip install -e .基本安裝 

下載V2進階模型，解壓縮到子目錄: checkpoints_v2

pip install git+https://github.com/myshell-ai/MeloTTS.git
python -m unidic download安裝  MeloTTS:

執行demo_part3.ipynb
目錄下的範例程式是.ipynb，所以要用Python Notebook來執行

pip install jupyterlab安裝Notebook程式 (jupyter lab)

執行jupyter lab，由browser開啟demo_part3.ipynb
jupyter lab


將自已的錄音檔拷貝到子目錄 ./resources
修改二處程式碼 ，逐一執行每一個cell
reference_speaker = 'resources/albert.m4a'
    #'ZH': "在這次vacation中，我們計畫去Paris欣賞埃菲爾鐵塔和盧浮宮的美景。",
    'ZH': "我爸爸很高很帥,我媽咪很beautiful,我妹妹runs fast, she is cute,我很喜歡玩Lego。",

但遇到錯誤 (ffmpeg相關)

修正ffmpeg語言轉檔程式問題

執行第三個cell就遇到問題，因為無法讀取及轉換錄音檔，問了Grok得到二個方法

先按Ctrl+C中斷jupyter lab


	conda install ffmpeg 方法1: 


方法2:直接由官網找到執行檔，拷貝到OpenVoice目錄下
https://evermeet.cx/ffmpeg/

	執行jupyter lab，由browser開啟demo_part3.ipynb
jupyter lab

逐一執行每一個cell，又發現其他問題
********************************************************************** Resource averaged_perceptron_tagger_eng not found. Please use the NLTK Downloader to obtain the resource: >>> import nltk >>> nltk.download('averaged_perceptron_tagger_eng')

自然語言工具包(NLTK)及缺失的套件安裝
一樣問Grok(或ChatGPT)…
在出現錯誤的cell前，加一個程式cell
import nltk
nltk.download('averaged_perceptron_tagger_eng')執行此程式cell，錯誤消失

執行下一個cell，看到正確執行的過程

成果展現
指定的各種語言，逐一地產生音檔

<img width="557" height="87" alt="image" src="https://github.com/user-attachments/assets/41b7e0b2-0dac-4c4f-bc96-f233be851303" />



最後在轉韓語時有套件缺失問題，先忽略

聽聽看各國語音的音檔(子目錄outputs_v2)

<img width="195" height="278" alt="image" src="https://github.com/user-attachments/assets/1df09536-aa6c-49be-b3db-46b47a48c48f" />



反思與收穫
在做語音複製的時候，我利用了先前的經驗，從conda開了一個名稱為OpenVoice 的虛擬環境，並用jupyter lab這個指令從網頁上開啟，用電腦端當作伺服器，先是遇到了忘記安裝checkpoints_v2的問題，之後順利下載就解決了，再來是ffmpeg轉檔程式出現問題，於是詢問AI後，用了兩行指令解決，最後成功輸出聲音，但我覺得畢竟只是一個開放模型，在程式步驟沒有很複雜的情況下，聲音也沒有很像，但中文還是可以有些端倪，我覺得有5成的相似，是否可以讓它模仿的更精確也可以當作未來的課題去探討。






參考資料
How to Clone Any Voice With AI | Tortoise-TTS Tutorial
免費TTS工具OpenVoice
https://github.com/myshell-ai/OpenVoice
安裝: https://github.com/myshell-ai/OpenVoice/blob/main/docs/USAGE.md
Chatgpt.com
Grok https://grok.com/
