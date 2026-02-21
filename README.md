# 従業員管理システム 開発仕様書（新人研修用）

## 1. システム概要
本システムは、ID/パスワードによる認証を行い、従業員情報の検索・登録・更新・削除（CRUD）を行うWebアプリケーションです。標準的な階層化アーキテクチャを学び、モダンなJava開発の基礎を習得することを目的とします。

## 2. アーキテクチャ

| レイヤー | 技術スタック | 備考 |
| :--- | :--- | :--- |
| **Language** | Java 21 | `record` クラス等を積極的に活用すること |
| **Framework** | Spring Boot 3.x | Spring Security, Spring Data JPA |
| **View** | Thymeleaf | HTML5 / Bootstrap 5 (CSS) |
| **Database** | PostgreSQL 15.5 | |



---

## 3. 画面一覧・機能要件

| 画面名 | パス | 機能概要 |
| :--- | :--- | :--- |
| **ログイン画面** | `/login` | ID・パスワードによる認証。失敗時はエラーメッセージを表示。 |
| **検索画面** | `/employees` | 従業員の一覧表示。氏名による部分一致検索。 |
| **登録画面** | `/employees/new` | 新規登録。入力チェック（必須、桁数、形式）を行う。 |
| **編集画面** | `/employees/{id}/edit` | 情報修正および削除。更新時は排他制御を確認する。 |
| **エラー画面** | `/error` | 認証エラー、権限不足、システムエラー時の共通遷移先。 |

---

## 4. データベース設計

### 4.1. users (システム利用者)
| カラム名 | 型 | 制約 | 説明 |
| :--- | :--- | :--- | :--- |
| `user_id` | VARCHAR(20) | PK | ログイン用ID |
| `password` | VARCHAR(255) | NOT NULL | ハッシュ化されたパスワード |
| `role` | VARCHAR(20) | NOT NULL | 権限（例: ROLE_ADMIN） |

### 4.2. employees (管理対象 従業員)
| カラム名 | 型 | 制約 | 説明 |
| :--- | :--- | :--- | :--- |
| `id` | SERIAL | PK | 従業員番号（自動採番） |
| `name` | VARCHAR(100) | NOT NULL | 氏名 |
| `email` | VARCHAR(255) | UNIQUE | メールアドレス |
| `department` | VARCHAR(50) | | 部署名 |
| `version` | INTEGER | NOT NULL | 楽観的排他制御用（@Version） |



---

## 5. 推奨パッケージ構造
Spring Bootの標準的な構成に従うこと。

```text
com.example.demo
 ├── controller    (リクエスト制御)
 ├── service       (ビジネスロジック・トランザクション)
 ├── repository    (DBアクセス・JPA)
 ├── entity        (DBテーブル定義)
 ├── dto           (画面間のデータ転送・record活用)
 └── config        (Security設定など)