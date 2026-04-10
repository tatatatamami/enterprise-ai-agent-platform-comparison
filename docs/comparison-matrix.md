# 詳細比較マトリクス：AWS AgentCore vs Microsoft Azure AI Foundry

> **対象読者**：IT アーキテクト、AI 推進担当者、プリセールスエンジニア  
> **目的**：エンタープライズ AI エージェント基盤選定における客観的な評価指標の提供

---

## 1. プラットフォーム概要

| 評価項目 | AWS AgentCore | Microsoft Azure AI Foundry |
|---|---|---|
| **正式名称** | Amazon Bedrock AgentCore | Microsoft Azure AI Foundry（旧 Azure AI Studio） |
| **提供形態** | マネージドサービス（Amazon Bedrock の一部） | 統合プラットフォーム（Azure AI サービス群） |
| **主要ユースケース** | クラウドネイティブな AI エージェント構築・実行 | エンタープライズ統合型 AI エージェント構築・運用 |
| **リリース状況** | GA（一般提供中）※機能により異なる | GA（一般提供中）※機能により異なる |
| **対応リージョン** | 主要 AWS リージョン | グローバル Azure リージョン（日本リージョン含む） |

---

## 2. AI モデルとランタイム

| 評価項目 | AWS AgentCore | Microsoft Azure AI Foundry |
|---|---|---|
| **対応 LLM** | Amazon Titan、Claude（Anthropic）、Llama、Mistral 等 | Azure OpenAI（GPT-4o、o1 等）、Llama、Mistral、Phi 等 |
| **独自モデル** | Amazon Titan（テキスト・埋め込み） | Microsoft Phi シリーズ（SLM）、Azure OpenAI 専用モデル |
| **マルチモデル対応** | ○（Bedrock 上の複数モデルを切替可能） | ○（モデルカタログから選択・デプロイ） |
| **ファインチューニング** | ○（一部モデルに対応） | ○（Azure OpenAI ファインチューニング対応） |
| **エージェントランタイム** | Bedrock AgentCore Runtime | Azure AI Foundry エージェントサービス |
| **マルチエージェント** | ○（エージェント間オーケストレーション） | ○（AutoGen、Semantic Kernel との統合） |

---

## 3. エンタープライズ統合

| 評価項目 | AWS AgentCore | Microsoft Azure AI Foundry |
|---|---|---|
| **ID・アクセス管理** | AWS IAM / AWS Organizations | **Microsoft Entra ID**（条件付きアクセス、MFA、RBAC） |
| **シングルサインオン** | AWS SSO（IAM Identity Center） | **Entra ID SSO**（既存 AD・ADFS との完全統合） |
| **監視・オブザーバビリティ** | Amazon CloudWatch、AWS X-Ray | **Azure Monitor**（メトリクス・ログ・アプリケーションインサイト） |
| **ログ管理** | CloudWatch Logs | **Azure Monitor ログ / Log Analytics ワークスペース** |
| **エンタープライズ検索** | Amazon Kendra / OpenSearch | **Azure AI Search**（RAG パイプライン、ベクター検索、ハイブリッド検索） |
| **データ分析基盤** | AWS Glue / Amazon Athena / Amazon Redshift | **Microsoft Fabric**（OneLake、データ統合・分析の統合基盤） |
| **ワークフロー自動化** | AWS Step Functions / Amazon EventBridge | **Azure Logic Apps**（GUI ベースのビジネスプロセス自動化、数百のコネクタ） |
| **データストア** | Amazon S3、DynamoDB、RDS | Azure Blob Storage、Cosmos DB、Azure SQL、Azure Data Lake |

### 統合評価サマリー

Microsoft Foundry はエンタープライズ向け Microsoft 製品群（Microsoft 365、Power Platform、Dynamics 365）との密接な連携を前提に設計されており、既存の Active Directory/Entra ID 環境を持つ組織においては統合コストを大幅に低減できます。AWS AgentCore は AWS エコシステム内での完結性に強みを持ちます。

---

## 4. セキュリティとコンプライアンス

| 評価項目 | AWS AgentCore | Microsoft Azure AI Foundry |
|---|---|---|
| **データ暗号化（保存時）** | AES-256（AWS KMS） | AES-256（Azure Key Vault）|
| **データ暗号化（転送時）** | TLS 1.2+ | TLS 1.2+ |
| **顧客管理キー（CMK）** | ○（AWS KMS） | ○（Azure Key Vault / CMK） |
| **プライベートネットワーク** | AWS PrivateLink / VPC エンドポイント | Azure Private Link / VNet 統合 |
| **コンプライアンス認証** | ISO 27001、SOC 2、PCI DSS、HIPAA 等 | ISO 27001、SOC 2、PCI DSS、HIPAA、**FedRAMP**、**GDPR**、**ISMS（日本）** 等 |
| **責任ある AI** | Amazon Bedrock Guardrails | **Azure AI Content Safety**、**Responsible AI ダッシュボード** |
| **データ主権** | リージョン選択により対応 | **データ境界（EU、日本等）**、Purview によるデータガバナンス |
| **ネットワーク分離** | VPC、セキュリティグループ | Azure VNet、ネットワークセキュリティグループ |

---

## 5. 開発者エクスペリエンス

| 評価項目 | AWS AgentCore | Microsoft Azure AI Foundry |
|---|---|---|
| **SDK・ライブラリ** | AWS SDK（Python、Java、.NET 等）、Boto3 | Azure SDK、**Semantic Kernel**（.NET・Python・Java）、**AutoGen** |
| **エージェントフレームワーク** | Bedrock AgentCore SDK、LangChain 対応 | Semantic Kernel、AutoGen、LangChain 対応 |
| **ローカル開発** | AWS CLI、SAM、localstack | Azure CLI、**VS Code 拡張**、Azure Developer CLI（azd） |
| **プロンプト管理** | PromptFlow（限定的） | **Azure AI Foundry ポータル**（プロンプトフロー統合） |
| **評価・テスト** | Bedrock モデル評価 | **AI Foundry 評価フレームワーク**（カスタム評価メトリクス対応） |
| **IDE 統合** | AWS Toolkit for VS Code | **GitHub Copilot**、**VS Code Azure 拡張**（深い統合） |
| **テンプレート・サンプル** | AWS ソリューションライブラリ | **Azure サンプルギャラリー**、Microsoft Learn |

---

## 6. 運用・MLOps

| 評価項目 | AWS AgentCore | Microsoft Azure AI Foundry |
|---|---|---|
| **デプロイ管理** | AWS CDK、CloudFormation、Terraform | **Azure DevOps**、GitHub Actions、Terraform、Bicep |
| **CI/CD パイプライン** | AWS CodePipeline / CodeBuild | **Azure DevOps Pipelines**、**GitHub Actions**（Microsoft 所有） |
| **モデルバージョン管理** | Amazon SageMaker（別サービス） | **Azure AI Foundry モデルレジストリ**（統合） |
| **A/B テスト・実験管理** | AWS Experiment Manager（SageMaker） | **Azure AI Foundry 実験管理**（統合） |
| **スケーリング** | 自動スケーリング（Bedrock マネージド） | 自動スケーリング（Azure マネージド） |
| **SLA** | 99.99%（Bedrock） | 99.9%～99.99%（サービスにより異なる） |
| **コスト可視化** | AWS Cost Explorer | **Azure Cost Management + Billing**、**AI Foundry コスト分析** |

---

## 7. メモリと状態管理

| 評価項目 | AWS AgentCore | Microsoft Azure AI Foundry |
|---|---|---|
| **会話履歴管理** | Bedrock AgentCore メモリ | Azure AI Foundry スレッド管理 |
| **長期メモリ** | ○（AgentCore メモリストア） | ○（Azure Cosmos DB 等との統合） |
| **セッション管理** | ○ | ○ |
| **外部ナレッジベース** | Amazon Bedrock Knowledge Bases | **Azure AI Search**（エンタープライズグレードの RAG） |

---

## 8. 価格モデル

| 評価項目 | AWS AgentCore | Microsoft Azure AI Foundry |
|---|---|---|
| **課金モデル** | トークン従量課金＋ API 呼び出し数 | トークン従量課金＋サービス利用時間 |
| **既存ライセンス活用** | AWS Enterprise Support | **Microsoft 365 / Azure EA（Enterprise Agreement）との統合** |
| **無料枠** | AWS 無料利用枠（一部） | Azure 無料クレジット（一部） |
| **予算管理** | AWS Budgets | **Azure Budgets**、**Azure Cost Management** |

> **注記**：詳細な価格は各社公式サイトをご確認ください。価格は地域・契約形態・利用量により異なります。

---

## 9. サポートとエコシステム

| 評価項目 | AWS AgentCore | Microsoft Azure AI Foundry |
|---|---|---|
| **エンタープライズサポート** | AWS Enterprise Support | **Microsoft Unified サポート** |
| **パートナーエコシステム** | AWS パートナーネットワーク（APN） | **Microsoft Partner Network（MPN）**、ISV パートナー |
| **トレーニング・認定** | AWS 認定資格 | **Microsoft 認定資格**（AI-102、DP-100 等） |
| **ドキュメント** | AWS ドキュメント | **Microsoft Learn**（日本語含む多言語対応） |
| **コミュニティ** | AWS re:Post、GitHub | **Microsoft Tech Community**、GitHub、Stack Overflow |

---

## 総合評価サマリー

| 評価軸 | AWS AgentCore | Microsoft Azure AI Foundry |
|---|---|---|
| AI モデルの多様性 | ★★★★☆ | ★★★★★ |
| エンタープライズ統合 | ★★★☆☆ | ★★★★★ |
| セキュリティ・コンプライアンス | ★★★★☆ | ★★★★★ |
| 開発者エクスペリエンス | ★★★★☆ | ★★★★★ |
| 既存 Microsoft 環境との親和性 | ★★☆☆☆ | ★★★★★ |
| 既存 AWS 環境との親和性 | ★★★★★ | ★★★☆☆ |
| 運用・MLOps | ★★★★☆ | ★★★★★ |
| コスト透明性 | ★★★★☆ | ★★★★☆ |

> ★の評価は相対的な傾向を示すものであり、絶対的な優劣を示すものではありません。組織の既存環境・要件に応じて最適解は異なります。

---

*[← README へ戻る](../README.md)*
