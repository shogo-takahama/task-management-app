# タスク管理アプリケーション (Task Management App)

個人のタスク進行を効率化するためのWebアプリケーションです。  
モダン開発現場の標準（設計・テスト・DevOps・ドキュメンテーション）を意識して開発しています。

---

## 🛠 技術スタック (Tech Stack)

### Frontend
- **Framework:** React 19
- **Language:** TypeScript
- **Build Tool:** Vite
- **UI & Styling:** React Icons *(Tailwind CSS 導入予定)*
- **Data Fetching & State:** Axios *(TanStack Query 導入予定)*

### Backend
- **Framework:** Spring Boot 4.1.1
- **Language:** Java 21
- **Security:** Spring Security (JWT Authentication)
- **Database Access:** Spring Data JPA
- **Validation:** Spring Validation (`@Valid` による DTO 入力検証)
- **API Specification:** springdoc-openapi (Swagger UI による仕様可視化)

### Testing
- **Backend Unit Test:** JUnit 5, Mockito (Service層のビジネスロジック・例外処理テスト)
- **Backend Integration Test:** MockMvc (Controller層のJWT認可・バリデーション・HTTPステータステスト)

### Database & Infrastructure
- **Database:** PostgreSQL (Neon) / H2 Database (Local/Test)
- **Container:** Docker / Docker Compose
- **CI/CD:** GitHub Actions (ビルド・Lint・自動テストワークフロー)
- **Hosting:** Vercel (Frontend) / Render (Backend)

---

## 💡 設計と開発のポイント

- **設計判断の明記 (ADR):** 技術選定の理由や背景を ADR (Architecture Decision Records) として記録し、設計の意図を明確化
- **JWTによる認証・データ隔離:** ステートレスなJWT認証の導入と、ログインユーザー単位でのアクセス制限
- **レイヤー別の自動テスト:** Service層の単体テスト（JUnit 5 / Mockito）および Controller層のHTTP・認証結合テスト（MockMvc）の導入
- **OpenAPI / Swagger による仕様共有:** バックエンドのAPI定義を Swagger UI で視覚化し、フロントエンド開発との連携を効率化
- **GitHub Actions による自動化:** Commit / PR 作成時の自動ビルド・テストの実行と、クラウド環境へのデプロイ

---

## 🏗 システムアーキテクチャ (Architecture)

```mermaid
graph TD
    subgraph Frontend [Vercel / Local]
        Client[React 19 Client / Vite]
    end

    subgraph Backend [Render / Local]
        API[Spring Boot 4 API Server]
        Sec[Spring Security / JWT Filter]
        Val[Spring Validation]
        Swagger[Swagger UI / OpenAPI]
    end

    subgraph Database [Neon / Local]
        DB[(PostgreSQL)]
    end

    Client -->|REST API / HTTPS (JWT Header)| Sec
    Sec --> Val
    Val --> API
    API -->|Spring Data JPA| DB
    API -.-> Swagger
```

---

## 🎯 技術選定の意思決定 (ADR)

重要な技術選定において ADR を作成し、選定理由やトレードオフを記録しています。

- [ADR 0001: 基本アーキテクチャおよび技術スタックの選定](./docs/adr/0001-architecture-selection.md)

---

## 🚀 ローカル開発環境の起動手順 (Local Setup)

### 1. 前提条件 (Prerequisites)
- Java 21 以上
- Node.js 18 以上 (Vite 実行・パッケージ管理用) / npm

### 2. バックエンドの起動 (Backend Setup)
```bash
# ローカル用プロファイルで起動 (H2インメモリDB使用)
./mvnw spring-boot:run -Dspring-boot.run.profiles=local
```
* API エンドポイント: `http://localhost:8080`
* Swagger UI: `http://localhost:8080/swagger-ui.html`

### 3. フロントエンドの起動 (Frontend Setup)
```bash
# 依存パッケージのインストール & 開発サーバー起動
npm install
npm run dev
```
* フロントエンド画面: `http://localhost:5173`

---

## 🧪 テスト戦略 & 品質管理 (Testing Strategy)

リファクタリングに強いコードベースを維持するため、テストコードの実装を徹底しています。

```bash
# バックエンドの全テスト実行
./mvnw test
```

- **Service層 (Unit Test):** `JUnit 5` + `Mockito` を使用し、DBや外部通信をモック化して正常系・異常系例外処理を高速検証。
- **Controller層 (Integration Test):** `MockMvc` を使用し、リクエスト検証、`@Valid` バリデーション、JWT認可ヘッダーチェックおよびレスポンスコード（200, 400, 401, 404 等）を検証。

---

## 📝 開発プロセス (Development Process)

- **GitHub Issues & Projects:** タスク単位でIssueを管理し、カンバン形式で進捗を可視化。
- **Git Feature Branch Workflow:** `feat/`, `docs/`, `fix/` 等のプレフィックスを用いたブランチ命名規則を徹底。
- **Pull Request & CI Integration:** PR作成時に GitHub Actions による自動テスト・ビルドをパスすることをマージ条件化。