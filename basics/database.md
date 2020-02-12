## 「資源」的位置

- 回顧到目前為止，
    - Web 伺服器的一種類型，接收 HTTP Request 後，透過程式進行處理，
      並動態地產生內容的是應用伺服器
    - API 的設計可以根據 Protocol 的不同，或使用 RESTful 的模式等，有多種風格
    - 「狀態」或「資源」等概念出現
- 雖然很單純的提出了「**資源**」的概念，但資源是什麼呢

---

- 如果是以前發送靜態檔案的 Web 伺服器的話，資源 ≒ 檔案
- 不過對於應用伺服器並不一定要將檔案與資源1對1對應
- 不管是 Unix 還是 Windows，其 **檔案系統; file system** 是有，
    - 樹狀的資料結構
    - 指定檔案用的 **路徑; path**
    - 建立時間、更新時間、byte size 等 meta data
    - 內容資料的保存
- 以上等等特徵本身就是個共通的、必不可少的系統，
  但要實作 Web 應用的話這樣還不夠

---

- 由於想作為資源保存的東西有很多種，因此要將資料的內容 **結構化**
    - 可以指定每筆資料為具有特定值的組合
        - 例如，服務的使用者資訊的話 `id, email, family_name, given_name` 等等
    - 可以拒絕不符合結構的資料輸入，以進行驗證、強制限制
        - 在原始的檔案系統中，檔案是任意的資料
- 基於結構化的資料內容，欲對其進行 **查詢; query** 或 **搜尋; search**
    - 如果是任何的複數檔案，很難直接找到包含所要求內容的檔案
        - Binary 檔案的話，必須知道解析其內部結構的方法
        - 文字檔案的話，依 **encoding** 不同讀取方式也不同
        - 如果檔案被 **加密** 過的話可能就沒辦法直接讀出來用
- 想要有能夠 **在時間與空間上高效率地（time and spatial efficiency）** 進行查詢與搜尋的系統
    - 「找出包含目的字串的文字檔案」之類的原始操作的話有許多種方法（如 **grep**）
        - 但是，只用 grep 的話是以 **線性時間; linear time** 執行
        - 也就是說隨著檔案數（行數）增加，完成搜尋的時間呈線性增長
    - 順帶一提，如果能 query 的話基本上會想要用統一的語法來簡單地撰寫，也會想要組合複雜的條件

---

## 資料庫

- 這個世界所需要的正是 **資料庫; database (DB)**
    - 這種需求是 **Web 誕生以前就有了**，有豐富的知識、研究與技術的積累
    - 豐富所以自然是個很難入門的世界
- 多數的應用伺服器都是將存在於某處的資源組合之後來實現功能，而 DB 就是其中主要存放的地方之一
- DB 管理系統（**database management system; DBMS**）也存在著許多種，大致的共通點的話，
    - 在檔案系統上執行，最後其資料實體以檔案的形式保存
    - 但是，保存（**永久化**）時的檔案格式與讀取時的格式未必相互對應。
      這邊只要在實作 DBMS 時知道就好，已經從使用者端 **隱藏** 了
        - **Database engine/Storage engine** 所負責的部分。與其 DBMS 的效能有密切的關係

---

- Application 所使用的資源要如何表達、如果將其（物理性地、邏輯性地）保存於 DB 之中已經被研究很長一段時間了
- **關聯（relational）資料模型; Relational data model** 與，
  與其對應的 **關聯式（relational）資料庫; Relational database management system, RDBMS** 已經被很廣泛地使用
    - RDBMS 透過在「一定程度上」被標準化的 **SQL** 這樣的專用語言（**Domain Specific Language; DSL**）來對資料進行操作或查詢
        - 關鍵是「一定程度上」，儘管標準組織已經制定了標準，
          對應的情況各不相同，很多產品有自己獨自的擴充
    - 能夠強制規定屬於特定空間或集合（**表; table**）的資料（**紀錄; record**）
      為具有特定值的組合（**Schema**）
    - 根據產品能夠執行像「**將一連串的操作，如果沒有成功（發生了些問題的時候）可以將其回復到什麼都沒發生的樣子**」
      這樣的 all-or-nothing 的操作
        - 這稱作 **交易; transaction** 處理
        - 滿足 Transaction 處理的所有特性被稱為 **ACID(Atomicity, Consistency, Isolation, Durability)**
        - 例如在 **電子商務; E-Commerce** 常見的例子，
          想「消費使用者持有的點數，讓商品可以使用」這樣的處理變成不可分割
            - 「只減少了點數，商品卻不能用」這樣的 **部分失敗; partial failure** 會嚴重損害使用者體驗
    - 有名的 RDBMS: **MySQL**, **PostgresSQL**, **Oracle Database** 等

---

- 另一方面，最近幾年開始出現光靠 RDBMS 沒辦法對 Web 應用的請求確保最佳的效能與可靠性的情況頻繁地出現
- 此時出現了叫做 **NoSQL; Not only SQL** 這樣的資料庫類型
    - 在接受或放棄現有 RDBMS 產品的某些功能的同時，
    - 採取另一種方法來提高效能和可靠性
- **Key-Value Store; KVS**
    - 「Key 所對應的值」，以這樣單純的配對為基礎來保存資料
    - （某種程度上）放棄複雜資料的結構化，或對其所使用複雜的 Query 等，取而代之
       使用高速且適用於 **分散; distributed** 處理的方式
    - 也有不將資料永久化，只在記憶體中實現的方式
    - 有名的 KVS: **Amazon Dynamo**, **RiakKV**, **Redis**(in-memory) 等

---

- **Document 導向 DB; Document-oriented database**
    - 能夠使用 **JSON** 等的以結構化的「 Document 」格式，但不被固定的 Schema 限制的資料
    - 也能夠在一個 Document 中對資料進行分層
    - 雖然沒有將 SQL 那樣被標準化，但很多產品都有各自用來查詢的語法
    - 雖然很多沒有支援 ACID Transaction，但有比較弱但可以用來保證的功能
        - **一致性; eventual consistency**
        - **樂觀鎖; optimistic lock/optimistic concurrency control**
    - 有名的 Document 導向 DB: **Amazon DynamoDB**, **MongoDB**, **CouchDB**, **MarkLogic** 等
    - **全文搜尋引擎; Full-text search engine** 也被視為 Document 導向 DB 的一種
- **列式資料庫; Columnar database**
- **圖資料庫; Graph database**

---

## 索引

- 不管是 RDBMS 還是 NoSQL，如果讀取資料所需時間程 **線性時間(O(N))** 的話就沒意義了
    - 例如，如果以使用者 ID 來取得使用者資訊的時候，
      若使用者總數有 100 萬人的話，以最壞情況比許遍歷全部 100 萬筆才能找到目標使用者，等等的對導致系統無法使用
- 幾乎所有 DB 為了讓資料取出時間不用達到線性時間都會多用些資料空間來存放所謂的 **索引; index**
    - 「資料所持有的項目（屬性）之一（**index key**）」，
      能夠快速得知「永久化的資料物理上的空間位置」
    - **平衡樹; Balanced tree** 的一種（**B-tree** 或 **B+ tree** 等）或
      以 **哈希表; Hash table** 實作
        - 例如[MongoDB 使用 B-tree](https://docs.mongodb.com/manual/indexes/index.html#id2)
    - 很多可以用 **O(log(N))** 的時間來進行取得、更新及刪除
    - 相同值的資料在集合內只能存在一筆（**單一性**; uniqueness）的限制，
      只要事先建構以目標值為 key 的 index 即可實現
- 若要「正確地」且「有效率地」使用 DB 的話**重要的是要意識到 Index 的存在**
    - 在設計讓 Application 存取的資料結構時，必須經常思考讓 DB 能夠有效率地使用這件事
