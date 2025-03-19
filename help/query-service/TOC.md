---
audience: user
user-guide-title: Adobe Experience Platform クエリサービスのヘルプ
breadcrumb-title: クエリサービスガイド
user-guide-description: 標準 SQL を使用して、Experience Platform でデータレイク内のデータをクエリします。
feature: Queries
role: User,Developer
source-git-commit: 9eee0f65c4aa46c61b699b734aba9fe2deb0f44a
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 71%

---


# Adobe Experience Platform クエリサービス {#query}

- [クエリサービスの概要](home.md)
- [クエリサービスのパッケージング](packaging.md)
- [クエリサービスガードレール](guardrails.md)
- はじめに {#get-started}
   - [前提条件](get-started/prerequisites.md)
- Data Distiller {#data-distiller}
   - [概要](data-distiller/overview.md)
   - [ライセンス使用状況](data-distiller/license-usage.md)
   - 派生データセット {#derived-datasets}
      - [概要](data-distiller/derived-datasets/overview.md)
      - [SQL を使用した派生データセットの作成](data-distiller/derived-datasets/create-derived-datasets-with-sql.md)
      - [デシルベースの派生データセットの作成](data-distiller/derived-datasets/decile-based-derived-attributes.md)
   - 拡張アプリケーション レポート用 SQL インサイト {#sql-insights}
      - [概要](data-distiller/sql-insights/overview.md)
      - [クエリプロモード](data-distiller/sql-insights/query-pro-mode.md)
      - [高速クエリの送信](data-distiller/sql-insights/send-accelerated-queries.md)
      - [レポートインサイトデータモデルガイド](data-distiller/sql-insights/reporting-insights-data-model.md)
   - の AI/ML 機能パイプライン {#ml-feature-pipelines}
      - [概要](data-distiller/ml-feature-pipelines/overview.md)
      - [Jupyter Notebooks への接続](data-distiller/ml-feature-pipelines/establish-connection.md)
      - [探索的データ分析](data-distiller/ml-feature-pipelines/exploratory-analysis.md)
      - [ML のエンジニア機能](data-distiller/ml-feature-pipelines/feature-engineering.md)
      - [ML 環境へのデータの書き出し](data-distiller/ml-feature-pipelines/export-data.md)
      - [AI/ML データパイプラインのエンリッチメントエンドツーエンドワークフロー](data-distiller/ml-feature-pipelines/end-to-end-notebook-workflow.md)
   - [Summit 2025 セッション](data-distiller/top-tips-to-maximize-value.md)
- Data Distillerの統計情報と機械学習 {#advanced-statistics}
   - [概要](advanced-statistics/overview.md)
   - [特徴エンジニアリング](advanced-statistics/feature-engineering.md)
   - [モデル](advanced-statistics/models.md)
   - [機能変換](advanced-statistics/feature-transformation.md)
   - モデルの実装 {#implement-models}
      - [モデルの実装](advanced-statistics/implement-models/implement-models.md)
      - [回帰](advanced-statistics/implement-models/regression.md)
      - [分類](advanced-statistics/implement-models/classification.md)
      - [クラスタリング](advanced-statistics/implement-models/clustering.md)
   - 例 {#examples}
      - [統計と機械学習を使用したボットフィルタリング](advanced-statistics/examples/statistics-and-ml-bot-filtering.md)
      - [SQL ベースのロジスティックリグレッションを使用して顧客のチャーンを予測](advanced-statistics/examples/predict-customer-churn.md)
- Data Distillerのオーディエンス {#data-distiller-audiences}
   - [SQL を使用した外部オーディエンスの構築](data-distiller-audiences/overview.md)
- 例 {#use-cases}
   - [概要](use-cases/overview.md)
   - [参照を中止](use-cases/abandoned-browse.md)
   - [属性分析](use-cases/attribution-analysis.md)
   - [ボットフィルタリング](use-cases/bot-filtering.md)
   - [統計と機械学習を使用したボットフィルタリングの概要](use-cases/statistics-and-ml-bot-filtering-stub.md)
   - [イベントのトレンドレポートの作成](use-cases/trended-report-of-events.md)
   - [同意分析](use-cases/consent-analysis.md)
   - [顧客のライフタイムバリュー](use-cases/customer-lifetime-value.md)
   - [データの調査](./use-cases/data-exploration.md)
   - [デシルベースの派生データセット](use-cases/deciles-use-case.md)
   - [あいまい一致](use-cases/fuzzy-match.md)
   - [ユーザーのページビューのリスト](use-cases/list-visitor-sessions.md)
   - [ページビュー別の訪問者のリスト](use-cases/visitors-by-number-of-page-views.md)
   - [SQL を使用して顧客チャーンを予測](use-cases/predict-customer-churn-stub.md)
   - [傾向スコア](use-cases/propensity-score.md)
   - [上位関数を持つ類似レコードの取得](use-cases/retrieve-similar-records.md)
   - [分析データからのマーチャンダイジング変数の返しと使用](use-cases/merchandising-variables.md)
   - [SQLAlchemy](use-cases/sqlalchemy.md)
   - [訪問者のロールアップレポートの表示](use-cases/roll-up-report-of-a-visitor.md)
   - [Web およびモバイル分析のインサイト](use-cases/analytics-insights.md)
- 重要な概念 {#key-concepts}
   - [ネストされたデータ構造の使用](key-concepts/nested-data-structures.md)
   - [ネストされたデータ構造の統合](key-concepts/flatten-nested-data.md)
   - [匿名ブロック](key-concepts/anonymous-block.md)
   - [インラインテンプレート](key-concepts/inline-templates.md)
   - [増分読み込み](key-concepts/incremental-load.md)
   - [データ重複排除](key-concepts/deduplication.md)
   - [データセットのサンプル](key-concepts/dataset-samples.md)
   - [データセット統計の計算](key-concepts/dataset-statistics.md)
- Data Distiller Hypercubes {#hypercubes}
   - [ハイパーキューブによる効率的なビッグデータ分析](hypercubes/overview.md)
- クエリサービスへのクライアントの接続 {#clients}
   - [クライアント接続の概要](clients/overview.md)
   - [SSL モード](./clients/ssl-modes.md)
   - [Aqua Data Studio](clients/aqua-data-studio.md)
   - [DbVisualizer](./clients/dbvisulaizer.md)
   - [GitHub コパイロット](./clients/github-copilot.md)
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
   - [パラメーター化クエリ](ui/parameterized-queries.md)
   - [クエリスケジュール](ui/query-schedules.md)
   - [クエリログ](ui/query-logs.md)
   - [スケジュール済みクエリの監視](ui/monitor-queries.md)
   - [資格情報ガイド](ui/credentials.md)
   - [クエリ結果からの出力データセットの生成](ui/create-datasets.md)
- クエリサービス API {#api}
   - [はじめに](api/getting-started.md)
   - [クエリ](api/queries.md)
   - [接続パラメーター](api/connection-parameters.md)
   - [スケジュール](api/scheduled-queries.md)
   - [スケジュールされたクエリの実行](api/runs-scheduled-queries.md)
   - [クエリテンプレート](api/query-templates.md)
   - [高速クエリ](api/accelerated-queries.md)
   - [アラート購読](api/alert-subscriptions.md)
- Data Distiller Authorization API {#auth-api}
   - [概要](auth-api/overview.md)
   - [はじめに](auth-api/getting-started.md)
   - [IP アクセス](auth-api/ip-access.md)
   - [検証](auth-api/validate.md)
- データガバナンス {#data-governance}
   - [概要](data-governance/overview.md)
   - [監査ログガイド](data-governance/audit-log-guide.md)
   - [アドホックスキーマデータセット内の ID](data-governance/ad-hoc-schema-identities.md)
   - [アドホックスキーマの属性ベースのアクセス制御のサポート](./data-governance/ad-hoc-schema-labels.md)
- ベストプラクティス {#best-practices}
   - [クエリの実行](best-practices/writing-queries.md)
   - [データアセット組織](./best-practices/organize-data-assets.md)
- SQL リファレンス {#sql}
   - [SQL の概要](sql/overview.md)
   - [SQL 構文](sql/syntax.md)
   - [アドビ定義関数](sql/adobe-defined-functions.md)
   - [高階関数](sql/higher-order-functions.md)
   - [Spark SQL 関数](sql/spark-sql-functions.md)
   - [メタデータコマンド](sql/metadata.md)
   - [準備済み文](sql/prepared-statements.md)
- [よくある質問](troubleshooting-guide.md)
- [IP アドレスの許可リスト](ip-address-allowlist.md)
- [API リファレンス](https://www.adobe.io/experience-platform-apis/references/query-service/)
- [Platform リリースノート](https://experienceleague.adobe.com/ja/docs/experience-platform/release-notes/latest)
