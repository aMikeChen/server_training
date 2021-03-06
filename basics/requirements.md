## 前提知識: Web 的介紹

- 在本文件中假設對於 "**Web (World Wide Web; WWW)**" 已經大略了解
    - 可以大略想像得到實現 Web 的種種技術
    - 就算沒受過系統化的教育，如果有關於 Web 是什麼東西的一般知識的話即可
    - 知道以下幾個關鍵字指的是什麼的話基本沒有問題
        - **TCP**
        - **HTTP**
        - **DNS**
        - **URI** / **URL**
        - **HTML**
- 假設也已知主從式架構 (**Client and Server**) 的概念

---

## 預設前提1: 能夠自己查

- 本文件並不以包山包海的教科書為目標，也沒辦法
- 因此前提是如果文章中有不明白的部分的話能夠 **自己查**
    - 不是「馬上」也沒關係，想到的時候再查也行。只要追加在腦中「未知領域」的列表即可。
    - 這個領域是個有趣的資料結構。cf. Algorithms and Data Structures; 演算法與資料結構
        - 不限於先進先出的順序 (**Queue**)，亦不限於先進後出的順序 (**Stack**)
        - 也不是以「重不重要」等等的指標來決定順序（**Heap**）
        - 常常會追加之後卻沒辦法再次取出
        - 甚至也可能 **追加之後卻不知不覺地消失了**

---

- 不需要什麼都瞬間連定義或公理都完全理解
    - 在那當下先針對有興趣的部分去挖掘
    - 在過程中一定會發現新的東西，如果有時間的話可以再找下去，沒有的話就 **延遲求值(Lazy evaluation)**
        -  先深入「未知領域」並按照同樣的流程
        - 「說起來之前好像也不懂這個單字呢」之類的「察覺」，也就是所謂的 **duplicated insert**
            - 雖是這樣不過不知道是不是嚴謹的有 **單一性(uniqueness)** 的 **集合(Set)**
            - 總之能夠透過各種機會重複地懷有疑問這件事應該跟對自己來說很重要的可能性有關
    - 只要這樣重複下去，自然地就能夠追溯到根本

---

- 像這樣照順序去查「未知的東西」是「最重要的技術」
    - 順帶一提，當必須要去調查的時候也有所謂的「戰略」
        - 如果優先考慮對當前主題的粗略了解的話就用 **廣度優先(Breadth-first)搜尋**
        - 如果想要更深入的話就用 **深度優先(Depth-first)搜尋**
            - 推薦在深夜的時候進行，上班就給他遲到

---

- 本文件想要傳達的事情之中，**其實有七成都是關於這個「能夠自己查」的重要性**
    - 說到底「Server 開發的基礎」只要大家能夠各自地去調查的話其實也沒有教的必要
        - **平行** 且 **非同步(Asynchronous)** 的形式
        - 相反的多人同時透過這份資料進行課堂演講的是 **同步** 處理
            - 每個人的進度都一樣
            - 如果在檢查每個人的理解程度的同時繼續進行，則整體的速度都會變成配合還不太理解的人
            - 另一方面，對於還不太理解的部分可能就說「之後再查就好」然後繼續下去
                - 這是「**將耗時的處理單元切換為非同步執行**」的行為。稱作 "Asynchronous job dispatch"
                - 如果將這個「之後再」唸成「需要的時候再」的話，也算是 **延遲求值**

---

- 剛開始在任何領域都會有很多還不知道的事物，且常常陷入「不知該從哪裡著手」的狀態
    - 就像不靠索引(**index**)地閱讀字典一樣。有些人會將字典從頭讀到尾。。。
- 本文件將關於 Server 開發的「這部分是常見/基礎/重要的」「這部分是關鍵」「這部分很有趣」等等的稍微用小故事來舉例。像是在字典上加上書籤一樣。
- 因此，使用方式會變成 **閱讀本文件時，請自行調查可能是重點的部分，並往下調查相關知識**
- 你可能已經注意到有幾個機制

---

- **粗體字** 或者與註記英文單字的部分是筆者認為重要的，或是「有興趣的話請調查」的部分
    - 特別是，請對像是專有名詞或專業用語之類的東西抱持興趣
    - 註記英文單字是因為英文的資料或有權威的資訊源比較多
- 基本上都不會特別加上連結。（雖然也是因為通通加上的話很麻煩，）
- 但 **確定可能的權威來源** 其實也是一門技術，希望可以練習看看
    - 確定「主要來源」
    - 雖是這樣不過其實也不難，只是需要習慣的程度。多試試看吧！
- 多次的**同樣的單字重複出現**，請試著來回檢查上下文。
    - 如果是以同樣的意思出現的話，
    - 應該會微妙地依上下文而有不太一樣的意思
        - 在軟體開發的世界是常有的事

---

## 「調查方法」的指引

- 廣泛地被使用的 Web 相關技術的話，
  很多會以 Internet Engineering Task Force; IETF 為中心，在業界相關的群組中進行討論之後，總結成**RFC(Request For Comment)**
    - 經過長時間的討論與公開的審查流程，通常都是權威性的資訊來源
        - 如此可以保證說明或定義無誤且可靠，
          不過請注意其內容在實際狀況中不一定是「好用」或「最佳」的
    - 例) [RFC 3986 - Uniform Resource Identifier (URI): Generic Syntax](https://tools.ietf.org/html/rfc3986)
    - 閱讀的時候，
        - "Status"或、
        - "Obsoletes: ..." 「廢止〜」(廢止並更換)或、
        - "Updates: ..." 注意有「更新〜」
    - 閱讀文件之前先確認文件有什麼改變。有可能是已經被廢止的版本。
    - **練習1**: 試著確定定義2018年5月目前有效的 HTTP version 1.1 標準規格的RFC（多份）。不限方法。
- 依事物的不同，其技術或概念可能會被寫成論文發表出來
    - **Google Scholar** 是搜尋這些論文非常好用的服務
    - 即使是那些未被同行審查期刊所接受的論文，也可以在 arXive 上發表

---

- 在調查一些 Application 或 Framework 的時候，基本上其開發者所提供的文件是 OK 的
    - 細微差異如下（接受異議）：
        - "Guide": 有順序地說明實際使用方法的文件
        - "Reference": 總結了 Application 的指令或 API 之詳細規格的文件
        - "Documentation": 介於 Guide 與 Reference 之間
- 若想一次知道某個領域的相關詞彙或概念的話，可利用書籍或相關網站
    - 因為書籍會有評論，很容易先判斷其好壞再購買。也可以向前輩指教
        - 尤其是在某種程度上已經過時且有標準聖經本的技術，直接閱讀會比較快
        - 新進員工訓練網站上有推薦的圖書集請多利用。圖書預算是確實有的
    - 說明網站、部落格等等的常常魚目混珠不可大意
        - 通常會附上權威性的資訊來源或相關文件的連結，在那之上再加入自己的詳細說明的文件，其內容是值得信賴的
            - 簡單來說書籍與論文一樣
        - 像 Wikipedia 之類的文章上如果有確實的附上 RFC 或論文的連結的話也是很有幫助的

---

- 重要的是，不論過程如何 **不吝於付出努力去找尋可信賴的資訊**
    - 常見情況1: 「將搜尋到的第一個網站大致看一下就覺得懂了」
    - 常見情況2: 「從 StackOverflow 複製貼上」
- 習慣之後，就能減少為了找尋可信賴的資訊的 **跳數(hop count)**
    - 將 RFC 或官方文件等等的主要資訊搜尋引擎化，使它能夠直接被搜尋
    - 追蹤那個領域的代表性研究者、開發者、開發企業的 SNS，或訂閱部落格，迅速地掌握最新資訊
    - 定期地改進過濾條件，以免被過多的資訊量淹沒

---

## 預設前提2: 實踐

- 雖然這邊強調「調查」這件事，不過當然**只是調查過的話對人類來說肯定還是會馬上忘記**
  如果不用一些方式動手去實踐的話是沒辦法將知識內在化(internalize)的
    - 雖然也可以透過多次反覆調查的方式去幫助記憶，但果然親身體會還是很重要的
    - 當開始採取行動的時候，必然會接收到外界的回饋，
      此時的焦慮與壓力是促進獲得技術與知識的動力
        - 羽生善治老師也有[「重圧を感ぜよ」と仰っている](http://www.hochi.co.jp/entertainment/20180430-OHT1T50154.html)
- 但是實務上所需要的內容的話，我認為可以自然地從實務來獲取學習的機會，所以是樂觀的
- 相反地、**不經常於業務上使用，但如果沒有一定程度的瞭解的話則會影響到服務與案件的整體設計、實作與運用的領域** 也很常見
    - 對自己來說，有意識地去追這些不同領域的知識，並時常地試著去撰寫程式是很重要的

好了，前面是很長的前言，終於可以進入 Server 的部分了
