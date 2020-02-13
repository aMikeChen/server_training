## 為何 Elixir

- 這個頁面的目標
  - 認識採用 Elixir 的理由
  - 大致了解公司內開發的框架 [Antikythera] 的目的
- 所需時間: 10 分鐘

---

- 伺服器端的開發可以使用許多語言或框架
  - C++
  - C#
  - Erlang
  - Java
  - Perl
  - PHP
  - Ruby
  - Python
  - Node.js
  - Scala
  - Elixir
  - Go
  - Rust
  - Haskell
  - ...

其中，IoT 事業部近年以 Elixir 為標準。

---

### 為何 ACCESS 的 IoT 事業部要選擇 Elixir 呢？

- Erlang VM 能夠同時執行多個「Application」
- 再者，在運行中的 VM 中，可以不用停止即可對個別的 Application 進行更新 (Hot Code Upgrade)
- 於是能夠建構出「共享同一個 Erlang VM 叢集，並將資源用於多個專案」這樣的環境
  - 此即為公司內開發 Elixir 製的 OSS 框架 - [Antikythera] 所欲達成的目標

[antikythera]: https://github.com/access-company/antikythera

---

- 在 [Antikythera] 之前，公司內部是個別對每個專案進行技術選擇、CI/CD 環境與執行環境的管理
  - 現在也多少有一些
- 這樣會發生什麼呢，
  - 所使用的語言、框架、套件以及這些的版本不一致或升級的不足
    - 知識、程式碼的共享變得困難
    - 由於負責的人繁忙、不在或離職所導致的遺留放置將導致安全上的風險
  - 除了實作基本的 Application 之外還有更多任務
    - 快速進行多個專案時的阻礙（Overhead）
    - 環境建立、基礎設施管理等其本身就很專業，
      稍不注意的話就會留下安全漏洞，或需要昂貴的成本
  - 將這些因素重疊後，Application 本身的品質則會降低
    - 人力與時間資源上的不足
    - 沒有自動化測試、持續提交等習慣

---

- Elixir 本身，
  - 有充實的官方工具鍊，
  - 語法功能也相對強大，卻又容易閱讀，
  - 再深入的話還有充實的高級功能可用來構建功能豐富的框架
- 以上等等具備著作為統一的後端語言所需要的功能
- 因此，從 2015 年開始 [Antikythera] 的開發，2016 年開始實際用於商業用途。
  最近如果沒有特殊的要求的話，基本上都使用 [Antikythera] 進行後端開發

---

- 也許你已經注意到，目前在公司內部 Elixir 的使用與 [Antikythera] 的使用實際上是不可分割的
  - 在準備好 [Antikythera] 的公司內部環境之前也有使用 Elixir 的標準 Web 框架 - 
    [Phoenix](https://github.com/phoenixframework/phoenix) 的專案，現在是沒有
  - 目前基本上不用 [Antikythera] 則沒有使用 Elixir 的理由，但**也不一定**，
    至少在公司內， [Antikythera] 提供了極大的便利
    - 無需準備專案個別的執行環境與 CI/CD 環境，可以立刻進行後端的開發與部署
    - 方便的通用功能（非同步作業執行機制、WebSocket 行程管理機制、標準加密函式庫等）
    - 由專門的團隊來負責依賴套件的版本管理或整體的安全性調查與對應等
    - 與用於發布靜態 Assets 的 CDN 串連
