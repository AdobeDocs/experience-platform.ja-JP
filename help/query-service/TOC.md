---
audience: user
user-guide-title: Adobe Experience Platform クエリサービスのヘルプ
breadcrumb-title: クエリサービスガイド
user-guide-description: 標準 SQL を使用して、Experience Platform でデータレイク内のデータをクエリします。
feature: Queries
source-git-commit: b8c2a9ab44274e2719e7178119a58f14d0442955
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 98%

---


# Adobe Experience Platform クエリサービス {#query}

- [クエリサービスの概要](home.md)
- [クエリサービスのパッケージング](packages.md)
- [クエリサービスガードレール](guardrails.md)
- Data Distiller {#data-distiller}
   - [ライセンスの使用](data-distiller/licence-usage.md)
- はじめに {#get-started}
   - [前提条件](get-started/prerequisites.md)
- ユースケース {#use-cases}
   - [参照を中止](use-cases/abandoned-browse.md)
   - [Adobe Target でのアクティビティ分析](use-cases/activity-analysis-with-adobe-target.md)
   - [属性分析](use-cases/attribution-analysis.md)
   - [ボットフィルタリング](use-cases/bot-filtering.md)
   - [Web およびモバイル分析のインサイト](use-cases/analytics-insights.md)
   - [傾向スコア](use-cases/propensity-score.md)
   - [SQLAlchemy](use-cases/sqlalchemy.md)
- クエリサービス API {#api}
   - [はじめに](api/getting-started.md)
   - [クエリ](api/queries.md)
   - [接続パラメーター](api/connection-parameters.md)
   - [スケジュール済みクエリ](api/scheduled-queries.md)
   - [スケジュールされたクエリの実行](api/runs-scheduled-queries.md)
   - [クエリテンプレート](api/query-templates.md)
   - [高速クエリ](api/accelerated-queries.md)
   - [アラート購読](api/alert-subscriptions.md)
- クエリサービス UI {#ui}
   - [UI の概要](ui/overview.md)
   - [クエリエディターユーザガイド](ui/user-guide.md)
   - [クエリテンプレート](ui/query-templates.md)
   - [クエリサービスの認証情報の使用](ui/credentials.md)
   - [クエリ結果からのデータセットの生成](ui/create-datasets.md)
- [クエリの監視](monitor-queries.md)
- クエリ高速化ストア{#query-accelerated-store}
   - [レポートインサイトデータモデル](query-accelerated-store/reporting-insights-data-model.md)
- ベストプラクティス {#best-practices}
   - [クエリ実行の一般的なガイダンス](best-practices/writing-queries.md)
   - [データアセット組織のガイダンス](./best-practices/organize-data-assets.md)
   - [ネストされたデータ構造の使用](best-practices/nested-data-structures.md)
   - [ネストされたデータ構造の統合](best-practices/flatten-nested-data.md)
   - [匿名ブロック](best-practices/anonymous-block.md)
   - [増分読み込み](best-practices/incremental-load.md)
   - [データ重複排除](best-practices/deduplication.md)
- 派生属性 {#derived-attributes}
   - [概要](derived-attributes/overview.md)
   - [十分位数のユースケース](derived-attributes/deciles-use-case.md)
- サンプルクエリ {#sample-queries}
   - [エクスペリエンスイベントクエリの例](sample-queries/experience-event.md)
   - [Adobe Analytics クエリの例](sample-queries/adobe-analytics.md)
- SQL リファレンス {#sql}
   - [SQL の概要](sql/overview.md)
   - [SQL 構文](sql/syntax.md)
   - [アドビ定義関数](sql/adobe-defined-functions.md)
   - [Spark SQL 関数](sql/spark-sql-functions.md)
   - [メタデータコマンド](sql/metadata.md)
   - [準備済み文](sql/prepared-statements.md)
   - [データセットのサンプル](sql/dataset-samples.md)
- クエリサービスへのクライアントの接続 {#clients}
   - [クライアント接続の概要](clients/overview.md)
   - [SSL モード](./clients/ssl-modes.md)
   - [Aqua Data Studio](clients/aqua-data-studio.md)
   - [DbVisualizer](./clients/dbvisulaizer.md)
   - [Jupyter Notebook](clients//jupyter-notebook.md)
   - [Looker](clients/looker.md)
   - [Postico](clients/postico.md)
   - [Power BI](clients/power-bi.md)
   - [PSQL](clients/psql.md)
   - [RStudio](clients/rstudio.md)
   - [Tableau](clients/tableau.md)
- データガバナンス {#data-governance}
   - [概要](data-governance/overview.md)
   - [監査ログガイド](data-governance/audit-log-guide.md)
   - [アドホックスキーマデータセット内の ID](data-governance/ad-hoc-schema-identities.md)
   - [アドホックスキーマの属性ベースのアクセス制御のサポート](./data-governance/ad-hoc-schema-labels.md)
- [トラブルシューティングガイド](troubleshooting-guide.md)
- [API リファレンス](https://www.adobe.io/experience-platform-apis/references/query-service/)
- [Platform リリースノート](https://experienceleague.adobe.com/docs/experience-platform/release-notes/latest.html?lang=ja)