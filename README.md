# AI-Driven Development Agents (`ai-driven-dev-agents`)

AI駆動開発（AIDD: AI-Driven Development）を実用レベルで自律稼働させるための、工程別専門エージェントチームの仕様・テンプレート集です。

Google Antigravity や Cursor、Claude Code、GitHub Copilot Workspace などのAIエージェント環境において、高品質な自律開発パイプラインを実現します。

---

## チーム概要

```mermaid
flowchart LR
    User([開発者 / PO]) <--> Orchestrator[<b>Lead Orchestrator</b><br/>全体統括・パイプライン管理]
    Orchestrator --> Spec[<b>Spec Designer</b><br/>仕様・受入基準 Gherkin]
    Spec --> Dev[<b>TDD Developer</b><br/>Red-Green-Refactor 実装]
    Dev <--> Critic[<b>Code Critic</b><br/>セキュリティ・エッジケース監査]
    Critic --> QA[<b>QA & E2E Verifier</b><br/>統合ビルド・E2Eテスト]
    QA --> Orchestrator
```

| エージェント | 役割 | 主な責務・アプローチ |
| :--- | :--- | :--- |
| [**Lead Orchestrator**](.agent/agents/lead-orchestrator.md) | 進行統括 | パイプライン制御、エージェント間の連携・差し戻し、人間との対話窓口 |
| [**Spec & Contract Designer**](.agent/agents/spec-designer.md) | 要件・仕様策定 | PRD作成、データ型・API仕様（OpenAPI）、受入基準（Given-When-Then） |
| [**TDD Developer**](.agent/agents/tdd-developer.md) | 実装・自己検証 | テスト駆動開発（Red-Green-Refactor）、全テストPassまでの自律ループ |
| [**Adversarial Code Critic**](.agent/agents/code-critic.md) | 批判的レビュー | OWASP Top 10セキュリティ監査、境界値・エッジケース、設計品質精査 |
| [**QA & E2E Verifier**](.agent/agents/qa-e2e-verifier.md) | 統合・E2E検証 | クリーンビルド、結合・E2Eテスト実行、マイグレーション検証、PR作成準備 |

---

## ディレクトリ構成

```text
ai-driven-dev-agents/
├── .agent/
│   ├── agents/                           # 各専門エージェントの定義・プロンプト
│   │   ├── lead-orchestrator.md          # 統括・オーケストレーター
│   │   ├── spec-designer.md              # 仕様策定・受入基準設計
│   │   ├── tdd-developer.md              # TDD実装・自己検証
│   │   ├── code-critic.md                # 批判的コードレビュー・セキュリティ
│   │   └── qa-e2e-verifier.md            # 統合検証・E2E
│   ├── specs/                            # 仕様書・設計テンプレート
│   │   └── template_prd_and_spec.md      # Gherkin受入基準付きPRDテンプレート
│   ├── checklists/                       # 各種品質チェックリスト
│   │   ├── critic_code_review_checklist.md  # レビュー・セキュリティチェックリスト
│   │   └── qa_e2e_checklist.md          # 統合QA・E2Eチェックリスト
│   └── README.md                         # エージェント運用ガイド
├── .gitignore
├── LICENSE
└── README.md
```

---

## 特徴とコア設計思想

1. **仕様書駆動（Spec-Driven）**
   * 実装エージェントに直接コードを書かせるのではなく、まず `spec-designer` が受入基準（Given-When-Then）を明確化。後続のハルシネーションや認識ズレを未然に防ぎます。
2. **生成と評価の分離（Generative vs Adversarial）**
   * 実装者とは別の批判的ペルソナを持つ `code-critic` を独立させ、自己正当化バイアスを排除した厳格なコードレビューを実施します。
3. **自律的TDDループ（Self-Healing Loop）**
   * `tdd-developer` はローカル環境でテストを実行し、テストが全件パスするまで自己修復ループを回してから納品します。
4. **最小限の人間介入（Human-in-the-Loop）**
   * 人間は「仕様確定時」と「PRマージ前」の2箇所だけレビューすれば良いため、開発スピードとガバナンスを両立できます。

---

## ライセンス

[MIT License](LICENSE)
