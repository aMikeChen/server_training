## API設計的風格

- 應用伺服器開發的大致流程為：
    - 規定 Request 的格式，
        - 這常被稱為 **API 規格; API specification**
    - 在實現規定的 Request 所對應的功能時，
    - 實作能夠與所規定的行為相同的程式
- 此時，要使用哪種 API 規格，**如果完全自己想的話很辛苦**。
    - 如果有一些原則的話，作為開發者會很開心，
    - 作為使用者端在不同服務之間有個大致共通的原則的話，也比較好理解
    - 能夠共享程式碼的可能性也會增加

---

- 有幾種 API 設計模式被提出，並根據用途與適性配合使用
- **RESTful**
    - 將「對資源的操作」，
    - 「透過在 HTTP 中定義好的 Method 來表示」，
    - 「**無狀態; Stateless**」（後述）的 Protocol 設計思想
    - 從嚴謹到鬆散的全部包含的話，其被使用於非常多的 Web API 上
        - Body 的格式（**IDL; Interface Description Language**）使用 **XML** 或 **JSON** 的比較多
    - 也有問題點
        - 受限的表達力。比較難用「資源」與「對其所作的操作」來表達的處理也很多
        - Body 的格式沒有與用來嚴格定義的 **Schema 語言; Schema Language** 綁定
            - 雖然每個的 IDL 還是存在，但可能難以使用，或表達力很低
            - 結果，API spec/documentation 的格式變的不一致，程式碼的自動產生變得很困難
        - 沒有統一的方法來組合多個流程的處理
            - 欲對多個資源的同時處理的情況，常需要發送多個 HTTP Request

---

- **grpc**
    - 在 **HTTP 2.0** 上實現的 Protocol
        - 在 HTTP 2.0 中可以只維持一個 TCP Connection，但在其中收發多個 HTTP Request
    - 使用 **Protocol Buffer (protobuf)** 作為 IDL。**與 Schema 語言綁定**
    - **RPC; Remote Procedure Call** 跟名字一樣就是往遠端伺服器發送指令的意思，以及它的格式
        - 叫做 RPC 的 Protocol 的情況:
            - 很多用在將應該在應用伺服器端執行的處理內容用更具體的方式表達的 Request 格式
            - 只用作將其他的 Protocol 用作轉發 RPC 的 Request (**transport protocol**)
                - 例) 在 grpc 中，雖然使用 HTTP 2.0 作為 transport protocol，
                  一旦連接建立之後，決定處理的內容最後還是用 protobuf 表示的 Body 其內容本身
                - 轉發路徑建立之後，就只用 body 的內容，URL 只有一個
                - Transport protocol 通常可以互換。HTTP 1.1/2.0、**Websocket**、Raw TCP
    - 正在逐漸普及

---

- **GraphQL**
    - 透過 Query 與 Mutation 這樣的框架來表達對資源操作的方法
    - 作為對 RESTful 所擁有的一些問題的解答，
        - 多個處理組合的表示、
        - Schema 語言、
        - 減少接收到的資訊等特點
    - 大多用實現於 HTTP 1.1 之上，不過這也是 RPC 介面的一種（在 body 表達全部），
      從原理上 transport protocol 是什麼都行
    - 正被使用於某些專案上

---

## 狀態、State

- 這單字已經在上面出現過了
- **有狀態的; Stateful** protocol 會在客戶端與伺服器之間依照一定的順序與程序進行資訊互換，
  整個處理是否正確是 **取決於直到當時處理的結果（＝狀態）**
    - 例如，TCP 是有狀態的
- **無狀態的** protocol 對每個 Request 都是獨立的，並不依存於直到當時的處理結果
    - HTTP 是無狀態的

---

- 基於 HTTP 的應用（API）層:
    - 大多時候會有像「登入狀態」這種表示，
        - 已完成使用者驗證，
        - 所得到有效期限內之認證 Token 的期間（**session**）之概念
    - Session 的控制不管怎樣在本質上都是 **有狀態的**
        - 伺服器端或客戶端其中一邊會需要狀態，不然每個 Request 都需要進行驗證
    - 除此之外也常有像 Application 是 **狀態的寶庫** 之類的像是
        - 在 **電子商務; E-Commerce** 系統，「購物車」或「戶頭」這樣的概念

---

- RESTful 的導引之一雖然有「無狀態的」，但要很好的做到是很難的
    - 對我自己來說在能幫助菜鳥理解的有以下的部落格
        - [技術/HTTP/REST設計思想の "Stateless" との付き合い方](https://www.glamenv-septzen.net/view/1350)
    - 將「**長期地永久化的資源之狀態**」，
      與「**短期地在客戶端與伺服器之間共享的 Application 的狀態**」分開來思考比較好
        - RESTful 中依 API 設計而分離的「狀態」基本上是後者
    - 這部分請在實際的產品開發上慢慢培養感覺
- 順便講一下，**冪等性; idempotency** 的概念在 API 設計中也很重要
