# エンタープライズ視点でのアーキテクチャ観点
## AWS AgentCore vs Microsoft Azure AI Foundry

> **対象読者**：エンタープライズアーキテクト、IT インフラ担当者、セキュリティ担当者  
> **目的**：AI エージェント基盤の設計・導入における重要なアーキテクチャ上の判断材料を提供する

---

## 1. アーキテクチャ設計思想の違い

### AWS AgentCore のアーキテクチャ思想

AWS AgentCore は **Amazon Bedrock** を基盤とするマネージドサービスとして設計されており、AWS のクラウドネイティブなエコシステム（S3、DynamoDB、Lambda、IAM 等）との連携を前提としています。エージェントのランタイム、ツール実行、メモリ管理が Bedrock の統合サービスとして提供されており、AWS 環境への依存を高める一方で、AWS 内での構築・実行においては高い完結性を持ちます。

**主な設計特性：**
- サービス指向のモジュール構成（各機能が独立した AWS サービスとして提供）
- Lambda 関数によるツール（Action）の実行
- S3・DynamoDB によるナレッジベースとメモリの永続化
- IAM によるきめ細かいアクセス制御

### Microsoft Azure AI Foundry のアーキテクチャ思想

Microsoft Azure AI Foundry は **エンタープライズ統合** を設計思想の中核に置いており、既存の組織インフラ（Active Directory/Entra ID、監視基盤、データ基盤、業務アプリケーション）との seamless な統合を実現することを目的として設計されています。

**主な設計特性：**
- エンタープライズ ID 基盤（Entra ID）との深いネイティブ統合
- Azure Monitor による包括的なオブザーバビリティ
- Microsoft 365・Power Platform・Dynamics 365 との連携
- Semantic Kernel / AutoGen による柔軟なエージェント設計

---

## 2. エンタープライズ統合アーキテクチャ

### 2-1. ID・アクセス管理（IAM）

#### Microsoft Entra ID 統合（Azure AI Foundry）

```
[エンドユーザー / 社員]
        │
        ▼
[Microsoft Entra ID]
 ・条件付きアクセスポリシー
 ・多要素認証（MFA）
 ・RBAC（ロールベースアクセス制御）
 ・既存 Active Directory との同期
        │
        ▼
[Azure AI Foundry エージェント]
 ・マネージド ID による資格情報管理
 ・Azure Key Vault によるシークレット管理
 ・監査ログの自動収集
```

**ポイント**：既存の企業 Active Directory 環境を持つ組織は、Entra ID Connect を通じて既存の認証基盤を Azure AI Foundry にそのまま拡張できます。新規の IAM 設計が不要であり、既存のセキュリティポリシー・条件付きアクセスルールが AI エージェントへのアクセスにも自動的に適用されます。

#### AWS IAM 統合（AgentCore）

```
[エンドユーザー / アプリケーション]
        │
        ▼
[AWS IAM / IAM Identity Center]
 ・IAM ロール・ポリシー
 ・SAML 2.0 フェデレーション
 ・AWS Organizations
        │
        ▼
[Bedrock AgentCore エージェント]
 ・実行ロールによる権限制御
 ・Secrets Manager によるシークレット管理
```

---

### 2-2. 監視・オブザーバビリティ

#### Azure Monitor 統合（Azure AI Foundry）

Microsoft Foundry は Azure Monitor との深い統合により、以下の包括的なオブザーバビリティを提供します。

```
[Azure AI Foundry エージェント]
        │
  ┌─────┴─────┐
  ▼           ▼
[Azure Monitor メトリクス]    [Azure Monitor ログ]
 ・レイテンシ                  ・会話ログ
 ・スループット                ・エラーログ
 ・エラーレート                ・監査ログ
        │                          │
        └─────────┬─────────────────┘
                  ▼
     [Application Insights]
      ・エンドツーエンドトレース
      ・パフォーマンスダッシュボード
      ・インテリジェントアラート
                  │
                  ▼
     [Log Analytics ワークスペース]
      ・KQL によるアドホック分析
      ・カスタムダッシュボード（Workbooks）
      ・Azure Sentinel との SIEM 連携
```

**エンタープライズ上のメリット**：既存の Azure Monitor 環境に AI エージェントの監視を追加するだけで、システム全体の統合監視ダッシュボードが実現します。個別の監視ツールを導入・管理する必要がなく、運用コストを削減できます。

---

### 2-3. エンタープライズデータへのアクセス（RAG パイプライン）

#### Azure AI Search 統合（Azure AI Foundry）

エンタープライズ環境における RAG（Retrieval-Augmented Generation）の実装において、Azure AI Search は以下の統合アーキテクチャを実現します。

```
[エンタープライズデータソース]
 ・SharePoint Online
 ・OneDrive for Business
 ・Azure Blob Storage
 ・Azure SQL / Cosmos DB
 ・社内データベース
        │
        ▼
[Azure AI Search インデクサー]
 ・自動クロール・インデックス更新
 ・統合ベクトル化（Azure OpenAI Embeddings）
 ・セマンティックランキング
 ・ハイブリッド検索（キーワード＋ベクター）
        │
        ▼
[Azure AI Foundry エージェント]
 ・RAG ツールとして Azure AI Search を呼び出し
 ・Entra ID によるドキュメントレベルの権限制御
 ・ユーザーが閲覧権限を持つデータのみ検索結果に表示
```

**差別化ポイント**：Azure AI Search は Entra ID との統合により、ユーザーの権限に応じて検索結果をフィルタリングする「セキュリティトリミング」機能を持ちます。AI エージェントが社内ドキュメントにアクセスする際も、既存のアクセス制御ポリシーが自動的に適用されます。

---

### 2-4. データ分析基盤との統合

#### Microsoft Fabric 統合

```
[Microsoft Fabric（OneLake）]
 ・組織全体のデータレイク
 ・ビジネスインテリジェンス（Power BI）
 ・データエンジニアリング（Spark）
 ・データウェアハウス（Synapse Analytics）
        │
        ▼
[Azure AI Foundry エージェント]
 ・Fabric データへの直接クエリ
 ・BI レポートの自然言語インタフェース
 ・データドリブンな意思決定支援
```

---

### 2-5. ビジネスプロセス自動化

#### Azure Logic Apps 統合

```
[ビジネストリガー]
 ・メール受信（Exchange Online）
 ・Teams メッセージ
 ・フォーム送信（Microsoft Forms）
 ・スケジュール実行
        │
        ▼
[Azure Logic Apps]
 ・数百以上のコネクタ（SAP、Salesforce、ServiceNow 等）※公式サイト参照
 ・GUI ベースのワークフロー設計
 ・エラーハンドリング・リトライ
        │
        ▼
[Azure AI Foundry エージェント]
 ・自然言語によるタスク処理
 ・構造化データの生成・変換
 ・後続アクションへの結果受け渡し
        │
        ▼
[後続システム]
 ・ERP・CRM への更新
 ・承認ワークフローの起動
 ・通知・レポート生成
```

---

## 3. セキュリティアーキテクチャ

### ゼロトラスト対応

Microsoft Foundry は **Microsoft のゼロトラストフレームワーク**（確認・最小権限・侵害前提）に基づき設計されています。

| ゼロトラスト原則 | Azure AI Foundry での実装 |
|---|---|
| **明示的な検証** | Entra ID による常時認証・認可（条件付きアクセス） |
| **最小権限アクセス** | Azure RBAC、マネージド ID、Just-In-Time アクセス |
| **侵害を前提とした設計** | Azure Sentinel、Microsoft Defender for Cloud との統合 |

### ネットワーク分離

```
[インターネット]
      │
      ▼
[Azure Front Door / Application Gateway]
 ・WAF（Web Application Firewall）
 ・DDoS Protection
      │
      ▼
[Azure Virtual Network（VNet）]
 ・プライベートサブネット
 ・Network Security Group（NSG）
      │
      ▼
[Azure Private Link]
 ・AI Foundry エンドポイントへのプライベート接続
 ・パブリックインターネットへの露出なし
      │
      ▼
[Azure AI Foundry エージェント]
 ・完全プライベート環境での実行
```

---

## 4. 継続的改善のアーキテクチャ（MLOps / LLMOps）

組織が AI エージェントを継続的に改善していくためのアーキテクチャ観点です。

### Azure AI Foundry の LLMOps サイクル

```
[1. 開発・実験]
 Azure AI Foundry ポータル
 ・プロンプトフロー設計
 ・モデル選択・設定
 ・初期評価
        │
        ▼
[2. 評価・テスト]
 AI Foundry 評価フレームワーク
 ・品質メトリクス（Ground Truth比較）
 ・安全性・公平性評価
 ・パフォーマンス負荷テスト
        │
        ▼
[3. デプロイ]
 Azure DevOps / GitHub Actions
 ・CI/CD パイプライン
 ・Blue-Green デプロイ / カナリアリリース
 ・IaC（Bicep / Terraform）
        │
        ▼
[4. 監視・フィードバック]
 Azure Monitor / Application Insights
 ・品質劣化の自動検出
 ・ユーザーフィードバック収集
 ・コスト・パフォーマンス監視
        │
        ▼
[1. 開発・実験（次サイクル）]
 収集データによるプロンプト・モデルの改善
```

---

## 5. 既存環境別の導入推奨パターン

| 現在の環境 | 推奨アプローチ |
|---|---|
| Microsoft 365 ＋ Azure AD 既存利用 | Azure AI Foundry（Entra ID 統合・既存資産活用） |
| SharePoint・Teams を業務利用 | Azure AI Foundry（Microsoft Graph API 連携） |
| Dynamics 365 / Power Platform 利用 | Azure AI Foundry（Power Automate・Logic Apps 統合） |
| AWS 環境のみ利用 | AWS AgentCore（エコシステム活用） |
| マルチクラウド環境 | 要件に応じた組み合わせ（API ゲートウェイによる抽象化） |

---

## 6. まとめ：アーキテクチャ上の重要な判断軸

エンタープライズ AI エージェント基盤の選定においては、以下のアーキテクチャ上の観点を重視することを推奨します。

1. **既存 ID 基盤との統合**：新規 IAM を構築するか、既存 Entra ID/Active Directory を拡張するか
2. **監視基盤の統一**：個別ツールを追加するか、既存監視基盤に組み込めるか
3. **データアクセス制御**：ユーザー権限に基づくきめ細かいデータアクセス制御が可能か
4. **ビジネスプロセスとの接続**：既存の業務システム・ワークフローとの統合容易性
5. **継続的改善の仕組み**：評価・デプロイ・監視の LLMOps サイクルが一貫して管理できるか

Microsoft Azure AI Foundry は特に **Microsoft 製品群を既存利用しているエンタープライズ組織** において、これらの観点でシームレスな統合アーキテクチャを実現します。

---

*[← README へ戻る](../README.md) | [比較マトリクスへ →](comparison-matrix.md)*
