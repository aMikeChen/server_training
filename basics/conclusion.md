## 總結: 長路漫漫，回首仍在山麓

- 綜上所述，我們稍微向伺服器開發領域踏出了一步
- 已經提到滿多相關知識了，不過沒有比較具體的例子，請各位再深入研究
- 在這之後將會花費很長的時間來實際撰寫伺服器端的程式，因此讓我們適當地回顧一下
- 再次強調，「**能夠自己查**」才是最重要的技術
- 不僅是伺服器，任何領域只在培訓的幾週內就要完全涵蓋是沒辦法的。我們仍在起點而已

---

## 附錄: 補充知識

- 在實務或這之後的培訓應該會接觸到，適當地查查看吧
- 應用伺服器的設計與實作的模式或思想
    - 我認為這部分最好與學習最新框架的同時一起學會比較好，因此沒有包含進去
    - 關鍵字是，**Model-View-Controller; MVC**, **Layered Architecture**, **Domain-driven Design; DDD** 等
        - 與手機應用或 Web 應用（這邊是指使用 JavaScript 實作並在瀏覽器上執行的 Application）的
          設計與實作的思想有共通也有不同的部分，需要注意一下
- 將資訊「從伺服器往客戶端」傳送的技術
    - 欲將資源的更新等某些資訊 **即時; realtime** 地通知客戶端的情況
    - 在維持 TCP Socket 的同時進行 **雙向通訊** 等。 cf. **Websocket**

---

- **微服務; Micro-service** 架構
    - 相對詞: **單體式; Monolithic** 架構
    - 個別地構建專用於特定用途的應用伺服器，並將這些組成整個伺服器端系統的方法
    - 容易進行部分性能的調整與更新、部分的故障不會影響到整體、與 **Container 技術** 有良好的兼容性等優點
- 由多個元件組成的伺服系統中的 **Pull** 型結構與 **Push** 型結構
    - Pull 型是「由客戶端去取得資源」，Push 型是「往客戶端推送資源」的印象
    - 雖與資源的來源與資源的使用者之間的數量關係有關，但不管什麼情況比較多是「**數量較少的負載較高**」
    - 例）我們通常會導入「Log 累積伺服器」之類的元素，但當有許多伺服器要向其發送（push）Log 時，會導致 Log 累積伺服器的負載變高。最壞情況下系統整體都會變得不穩定
        - cf. **單點故障; Single Point of Failure, SPOF**

---

## 附錄: 練習回顧

- 練習1: HTTP 1.1 的 RFC
    - 目前有效的如下:
        1. "Message Syntax and Routing" [RFC7230]
        2. "Semantics and Content" [RFC7231]
        3. "Conditional Requests" [RFC7232]
        4. "Range Requests" [RFC7233]
        5. "Caching" [RFC7234]
        6. "Authentication" [RFC7235]
    - 一開始的[RFC7230]有總結，也描述了與其他相關文件的關係
- 練習2: HTTP Methods
    - 歸納在[RFC7231]的[Section 4](https://tools.ietf.org/html/rfc7231#section-4)
    - 只有 `PATCH` method 是定義在[RFC5789](https://tools.ietf.org/html/rfc5789)

[RFC7230]: https://tools.ietf.org/html/rfc7230
[RFC7231]: https://tools.ietf.org/html/rfc7231
[RFC7232]: https://tools.ietf.org/html/rfc7232
[RFC7233]: https://tools.ietf.org/html/rfc7233
[RFC7234]: https://tools.ietf.org/html/rfc7234
[RFC7235]: https://tools.ietf.org/html/rfc7235
