---
audience: user
user-guide-title: Adobe Experience Platform クエリサービスのヘルプ
breadcrumb-title: クエリサービスガイド
user-guide-description: 標準 SQL を使用して、Experience Platform でデータレイク内のデータをクエリします。
feature: Queries
source-git-commit: 9fe547e6270b4ede3151158e3b474f8c3dfd2297
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 97%

---


# Adobe Experience Platform クエリサービス {#query}

- [クエリサービスの概要](home.md)
- [クエリサービスのパッケージング](packages.md)
- [クエリサービスガードレール](guardrails.md)
- はじめに {#get-started}
   - [前提条件](get-started/prerequisites.md)
- Data Distiller {#data-distiller}
   - [概要](data-distiller/overview.md)
   - [ライセンス使用状況](data-distiller/license-usage.md)
   - クエリ高速化ストア {#query-accelerated-store}
      - [高速クエリの送信](data-distiller/query-accelerated-store/send-accelerated-queries.md)
      - [レポートインサイトデータモデルガイド](data-distiller/query-accelerated-store/reporting-insights-data-model.md)
   - 派生属性 {#derived-attributes}
      - [概要](data-distiller/derived-attributes/overview.md)
      - [シームレスな SQL フロー](data-distiller/derived-attributes/seamless-sql-flow.md)
      - [デシルベースの派生属性の作成](data-distiller/derived-attributes/decile-based-derived-attributes.md)
- ユースケース {#use-cases}
   - [参照を中止](use-cases/abandoned-browse.md)
   - [属性分析](use-cases/attribution-analysis.md)
   - [ボットフィルタリング](use-cases/bot-filtering.md)
   - [イベントのトレンドレポートの作成](use-cases/trended-report-of-events.md)
   - [顧客のライフタイムバリュー](use-cases/customer-lifetime-value.md)
   - [デシルベースの派生属性](use-cases/deciles-use-case.md)
   - [あいまい一致](use-cases/fuzzy-match.md)
   - [ユーザーのページビューのリスト](use-cases/list-visitor-sessions.md)
   - [ページビュー別の訪問者のリスト](use-cases/visitors-by-number-of-page-views.md)
   - [傾向スコア](use-cases/propensity-score.md)
   - [SQLAlchemy](use-cases/sqlalchemy.md)
   - [分析データからのマーチャンダイジング変数の返しと使用](use-cases/merchandising-variables.md)
   - [訪問者のロールアップレポートの表示](use-cases/roll-up-report-of-a-visitor.md)
   - [Web およびモバイル分析のインサイト](use-cases/analytics-insights.md)
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
- クエリサービス UI {#ui}
   - [UI の概要](ui/overview.md)
   - [クエリエディターユーザガイド](ui/user-guide.md)
   - [クエリテンプレート](ui/query-templates.md)
   - [パラメーター化されたクエリ（制限）](ui/parameterized-queries.md)
   - [クエリスケジュール](ui/query-schedules.md)
   - [クエリログ](ui/query-logs.md)
   - [スケジュール済みクエリの監視](ui/monitor-queries.md)
   - [資格情報ガイド](ui/credentials.md)
   - [クエリ結果からの出力データセットの生成](ui/create-datasets.md)
- Query Service API エンドポイント {#api}
   - [はじめに](api/getting-started.md)
   - [クエリ](api/queries.md)
   - [接続パラメーター](api/connection-parameters.md)
   - [スケジュール](api/scheduled-queries.md)
   - [スケジュールされたクエリの実行](api/runs-scheduled-queries.md)
   - [クエリテンプレート](api/query-templates.md)
   - [高速クエリ](api/accelerated-queries.md)
   - [アラート購読](api/alert-subscriptions.md)
- データガバナンス {#data-governance}
   - [概要](data-governance/overview.md)
   - [監査ログガイド](data-governance/audit-log-guide.md)
   - [アドホックスキーマデータセット内の ID](data-governance/ad-hoc-schema-identities.md)
   - [アドホックスキーマの属性ベースのアクセス制御のサポート](./data-governance/ad-hoc-schema-labels.md)
- ベストプラクティス {#best-practices}
   - [クエリの実行](best-practices/writing-queries.md)
   - [データアセット組織](./best-practices/organize-data-assets.md)
- 基本的な概念 {#essential-concepts}
   - [ネストされたデータ構造の使用](essential-concepts/nested-data-structures.md)
   - [ネストされたデータ構造の統合](essential-concepts/flatten-nested-data.md)
   - [匿名ブロック](essential-concepts/anonymous-block.md)
   - [インラインテンプレート](essential-concepts/inline-templates.md)
   - [増分読み込み](essential-concepts/incremental-load.md)
   - [データ重複排除](essential-concepts/deduplication.md)
   - [データセットのサンプル](essential-concepts/dataset-samples.md)
   - [データセット統計の計算](essential-concepts/dataset-statistics.md)
- SQL リファレンス {#sql}
   - [SQL の概要](sql/overview.md)
   - [SQL 構文](sql/syntax.md)
   - [アドビ定義関数](sql/adobe-defined-functions.md)
   - [Spark SQL 関数](sql/spark-sql-functions.md)
   - [メタデータコマンド](sql/metadata.md)
   - [準備済み文](sql/prepared-statements.md)
- [よくある質問](troubleshooting-guide.md)
- [API リファレンス](https://www.adobe.io/experience-platform-apis/references/query-service/)
- [Platform リリースノート](https://docs.adobe.com/content/help/ja-JP/experience-platform/release-notes/latest.html)
