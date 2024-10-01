---
title: Adobe Experience Platform リリースノート 2024年9月
description: Adobe Experience Platform の 2024年9月のリリースノート。
source-git-commit: a342f38f09b84ef720d6135bc555844df12ee251
workflow-type: tm+mt
source-wordcount: '2199'
ht-degree: 23%

---

# Adobe Experience Platform リリースノート

**リリース日：2024年9月24日（PT）**

Adobe Experience Platformの既存の機能およびドキュメントのアップデート：

- [アラート {#alerts}](#alerts)
- [ダッシュボード {#dashboards}](#dashboards)
- [Data Prep {#data-prep}](#data-prep)
- [宛先 {#destinations}](#destinations)
- [エクスペリエンスデータモデル（XDM）{#xdm}](#xdm)
- [ID サービス {#identity-service}](#identity-service)
- [クエリサービス {#query-service}](#query-service)
- [Segmentation Service {#segmentation-service}](#segmentation-service)
- [ソース {#sources}](#sources)

## アラート {#alerts}

Experience Platform では、様々な Platform アクティビティに関するイベントベースのアラートを登録できます。Platform ユーザーインターフェイスの「[!UICONTROL アラート]」タブを使用して、様々なアラートルールを購読し、UI 内または電子メール通知を通じてアラートメッセージを受け取るように選択できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 開発用サンドボックスのサポート | 実稼動用サンドボックスと開発用サンドボックスの両方で [ アラートを登録 ](../../observability/alerts/ui.md) できるようになり、すべての環境でシームレスな監視が可能になります。 |
| メールテンプレート | [ メールアラート ](../../observability/alerts/ui.md) に詳細なアセット情報が含まれるようになり、重要な詳細をすぐに確認できます。 |
| カスタマイズの強化 | [ アラートしきい値 ](../../observability/alerts/ui.md#alert-threshold) を設定できるようになり、次のアラートタイプの特定のニーズに合わせてアラートを柔軟にカスタマイズできます。<br><ul><li>セグメントジョブの遅延</li><li>セグメントエクスポートの遅延</li><li>宛先フロー実行遅延</li><li>ID サービスフロー実行遅延</li><li>プロファイルフロー実行の遅延</li><li>ソースフロー実行遅延</li><li>クエリ実行遅延</li><li>アクティベーションスキップ率を超えています</li><li>ソース取り込みエラー率を超過</ul> |
| 展開されたアラート | 監査イベント情報アラートが、次の [ アラートルール ](../../observability/alerts/rules.md):<br> の購読で使用できるようになりました<ul><li>オーディエンス作成</li><li>オーディエンスの更新</li><li>オーディエンスの削除</li><li>データセットの作成</li><li>データセットの更新</li><li>データセット削除</li><li>スキーマ作成</li><li>スキーマの更新</li><li>スキーマを削除しました。 |

{style="table-layout:auto"}

アラートについて詳しくは、[[!DNL Observability Insights]  概要 ](../../observability/home.md) を参照してください。

## ダッシュボード {#dashboards}

Experience Platformでは、毎日のスナップショットで得られた、組織のデータに関する重要なインサイトを表示できる複数のダッシュボードを提供しています。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| ライセンス使用状況アドオンテーブル | コア製品とアドオン用の専用テーブルを使用して、ライセンスの使用状況を詳細に把握し、Platform リソースを管理できます。 サンドボックスレベルでのドリルスルー表示を使用して、各コア製品の主要指標を追跡および分析します。 アドオン指標は、コア製品指標とシームレスに統合され、使用状況の包括的なビューを提供します。 可視性の強化により、ライセンス管理を最適化し、組織のニーズに合わせてリソースを調整できます。 詳しくは、『 [[!UICONTROL  ライセンスの使用状況 ] ダッシュボードガイド ](../../dashboards/guides/license-usage.md#overview-tab) を参照してください。 |
| Query Pro モード – グローバルフィルターのアップグレード | Query Pro モードの新しい日付フィルターにより、分析が強化されました。 SQL クエリの動的な日付パラメーターを使用してインサイトを絞り込み、特定の時間枠でデータをフィルタリングします。 直感的な UI でプリセットまたはカスタムの日付範囲を選択し、すべてのユーザーに関連するダッシュボードを維持します。 ワークフローを簡素化し、精度を維持し、タイムリーに決定を下すことができます。 詳しくは、[ 日付フィルターの作成に関するガイド ](../../dashboards/data-distiller/query-pro-mode/filters/global-filter.md) を参照してください。 |
| Query Pro モード – ドリルスルー | Query Pro モードのドリルスルー機能でより深いインサイトを引き出し、高レベルのグラフから詳細なダッシュボードにシームレスに移動します。 この機能を使用すると、要約から詳細な分析に簡単に移行し、トレンド、顧客の行動、KPI を調べることができます。 自動フィルターのパススルーとマルチレベルのドリルスルーにより、データの一貫性が維持され、スムーズな探索が保証されます。 ワークフローを簡素化し、コンテキストを維持し、意思決定を迅速化します。 詳しくは、[ ドリルスルーの作成手順ガイド ](../../dashboards/data-distiller/query-pro-mode/drill-through.md) を参照してください。 |
| Query Pro モード – 高度なテーブル属性 | Query Pro モードの高度なテーブル属性を使用すると、データのビジュアライゼーションを効率化し、ワークフローの効率を高め、データを明確にすることができます。 カスタムダッシュボードから直接、テーブルに自動ソート、サイズ変更、ページネーションを追加します。 主要なデータの優先順位を付けるための列の並べ替え、最適な読みやすさを実現するためのサイズ変更、SQL クエリを変更しない大規模なデータセットのシームレスな移動を行います。 これらの機能を統合し、データインサイトを高める方法については、「[ 詳細を表示 ](../../dashboards/data-distiller/query-pro-mode/view-more.md)」ガイドを参照してください。 |
| 合計データ量 | 「平均プロファイル充実度」指標は、「合計データ量」指標に置き換えられました。 合計データ量は、エンゲージメントワークフローおよびパーソナライゼーションワークフローのために、リアルタイム顧客プロファイルで使用できる使用可能なデータの総量を指します。 この変更について詳しくは、[ 合計データボリュームガイド ](../../landing/license-usage-and-guardrails/total-data-volume.md) を参照してください。 |

{style="table-layout:auto"}

アクセス権限の付与方法やカスタムウィジェットの作成方法など、ダッシュボードの詳細については、まず[ダッシュボードの概要](../../dashboards/home.md)を参照してください。

## データ準備 {#data-prep}

データ準備を使用して、エクスペリエンスデータモデル（XDM）との間でデータのマッピング、変換、検証を行う。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!BADGE Beta]{type=Informative} 宛先で使用する新しい Data Prep 関数 | 宛先のユースケースに対して、次の配列関数を使用できるようになりました。<ul><li>`array_to_string`</li><li>`filterArray`</li><li>`transformArray`</li><li>`flattenArray`</li></ul> 詳しくは、[ データ準備関数ガイド ](../../data-prep/functions.md#arrays) を参照してください。 |

{style="table-layout:auto"}

データ準備について詳しくは、[ データ準備の概要 ](../../data-prep/home.md) を参照してください。

## 宛先 {#destinations}

**更新日：2024 年 9 月 30 日（PT）**

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先または更新された宛先** {#new-updated-destinations}

| 宛先 | 説明 |
| --- | --- |
| [Amazon広告 ](/help/destinations/catalog/advertising/amazon-ads.md) | 2024 年 9 月リリースでは、`countryCode` パラメーターをAmazon Ads に書き出すマッピングオプションが追加されました。 [ マッピング手順 ](/help/destinations/catalog/advertising/amazon-ads.md#map) で `countryCode` を使用して、Amazonでの ID 一致率を改善します。 |
| [[!BADGE B2B]{type=Informative} Demandbase](/help/destinations/catalog/advertising/demandbase.md) | この宛先を使用して、Account-Based Marketing（ABM）のユースケースのアカウントオーディエンスをアクティベートします。 DemandBase の B2BDemand Side Platform（DSP）を使用して、ターゲットアカウント内の関連するペルソナやロールにアドバタイズします。 Target アカウントは、マーケティングや販売におけるその他のダウンストリームのユースケース向けに、Demandbase のサードパーティデータを使用して強化することもできます。 |

{style="table-layout:auto"}

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| --- | --- |
| [ データセットの書き出し ](/help/destinations/ui/export-datasets.md) 機能強化 | 2024 年 9 月リリースのExperience Platformには、様々なデータエグレスのユースケースをより適切にサポートするために、データセット書き出し機能にいくつかの機能強化が含まれています。 次のような機能が強化されました。 <ul><li>サブフォルダーの追加と削除のオプションを含む、新しいデータフォルダー設定オプション。</li><li>完全ファイル書き出し（1 回）や終了日の指定機能を含む、新しい書き出しオプション</li><li>メモ：Adobeでは、9 月のリリースより前に作成されたすべてのデータセット書き出しデータフローに対して、2025 年 5 月 1 日のデフォルトの終了日も導入します。 これらのデータフローのいずれについても、顧客は終了日より前にデータフローの終了日を手動で更新する必要があります。そうしないと、書き出しはこの日に停止します。</li></ul> <br> ![ スケジュール設定手順で「スケジュールとフォルダーを編集」オプションを強調表示したExperience Platformユーザーインターフェイスの画像。](../2024/assets/september/edit-schedule-folders.png " スケジュール設定手順の「スケジュールとフォルダーを編集」オプション "){width="250" align="center" zoomable="yes"} |

{style="table-layout:auto"}

詳しくは、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているので、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客アクションから有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| スキーマエディターの機能強化 | スキーマエディターで更新された関係ワークフローを使用して、スキーマの関係を制御します。 既存の関係をExperience PlatformUI から直接簡単に更新または削除でき、スキーマ管理をよりスムーズかつ直感的にします。 参照スキーマを調整し、自信を持って関係の名前を変更して、セグメント化やその他の主要なプロセス全体でシームレスなデータ整合性を確保します。 スキーマ関係の効率的な管理について詳しくは、[UI での関係フィールドの定義 ](../../xdm/tutorials/relationship-ui.md#create-a-relationship-field-group) および [B2B 関係の定義 ](../../xdm/tutorials/relationship-b2b.md#edit-a-b2b-schema-relationship) に関するガイドを参照してください。 |

{style="table-layout:auto"}

XDM について詳しくは、[XDM システムの概要 ](../../xdm/home.md) を参照してください。

## ID サービス {#identity-service}

Adobe Experience Platform ID サービスを使用すると、デバイスやシステム間で ID を紐付けることで、顧客とその行動の全体像を把握し、インパクトのある、個人的なデジタルエクスペリエンスをリアルタイムで提供できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| ID グラフリンクルールの限定提供 | ID グラフリンクルールは、ユーザーに正確なパーソナライゼーションを提供するために使用できる、ID サービスのツールスイートです。 <ul><li>[ID 最適化アルゴリズム ](../../identity-service/identity-graph-linking-rules/identity-optimization-algorithm.md) を使用して、ID グラフが単一の人物を表していることを確認し、リアルタイム顧客プロファイルでの不要な ID の結合を防ぐことができるようになりました。</li><li>[ 名前空間の優先度 ](../../identity-service/identity-graph-linking-rules/namespace-priority.md) を設定して、それぞれの名前空間の重要性を定義し、プロファイルの形成およびセグメント化の方法に影響を与えます。</li><li>UI の [ グラフシミュレーションツール ](../../identity-service/identity-graph-linking-rules/graph-simulation.md) を使用して、様々な設定の ID グラフをシミュレーションします。</li><li>[ID 設定インターフェイス ](../../identity-service/identity-graph-linking-rules/identity-settings-ui.md) を使用して、一意の名前空間を指定し、組織内のすべての名前空間の優先度を確立します。</li><li>グラフデータに関する指標と傾向については、[ID ダッシュボード ](../../identity-service/identity-graph-linking-rules/implementation-guide.md#validate-your-graphs) を参照してください。</li></ul> ID グラフのリンクルールを試すには、Adobeアカウントチームに連絡して、開発用サンドボックスへのアクセス権を確認します。 |

**更新されたドキュメント**

| 機能 | 説明 |
| --- | --- |
| ID グラフリンクルールのトラブルシューティングガイド | ID グラフリンクルールに関する新しい [ トラブルシューティングガイド ](../../identity-service/identity-graph-linking-rules/troubleshooting.md) を参照して、ID グラフリンクルールを使用する際に発生する可能性のある一般的な問題を解決するためのアプローチとデバッグソリューションを確認します。 |
| ID グラフリンクルールに関する FAQ | 名前空間の優先度、ID 最適化アルゴリズム、ID グラフリンクルールのその他のファセットに関するよくある質問への回答のリストについては、新しい [ID グラフリンクルールに関する FAQ](../../identity-service/identity-graph-linking-rules/troubleshooting.md#frequently-asked-questions) を参照してください。 |

{style="table-layout:auto"}

ID サービスについて詳しくは、[ID サービスの概要](../../identity-service/home.md)を参照してください。

## クエリサービス {#query-service}

クエリサービスを使用すると、標準 SQL を使用して Adobe Experience Platform [!DNL data lake] でデータに対してクエリを実行できます。データレイクの任意のデータセットを結合したり、クエリ結果を新しいデータセットとして取得したりすることで、それらのデータセットをレポートやデータサイエンスワークスペースで使用したり、リアルタイム顧客プロファイルに取り込んだりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| Data Distiller オーディエンス | Experience Platformの Data Distillerの SQL オーディエンス拡張機能を使用すると、オーディエンスを簡単に作成、管理、アクティブ化できます。 プロファイルに生データを必要とすることなく、データレイクから直接 SQL コマンドを使用してオーディエンスセグメントを定義します。 この柔軟でデータ駆動型のアプローチにより、ターゲティング戦略を絞り込み、オーディエンスをファイルベースの宛先に自動的に同期します。 ワークフローを合理化し、オーディエンス管理を最適化し、データの可能性を最大限に活用します。 [SQL オーディエンス拡張機能の使用に関するガイド ](../../query-service/data-distiller-audiences/overview.md) を参照して、オーディエンス戦略を強化します。 |
| Data Distiller Statistics - ハイパーキューブ | ハイパーキューブを使用してビッグデータ分析を最適化します。 履歴データを再処理することなく、個別カウントや多次元分析などの複雑な計算を処理できます。 精度と効率を維持しながら、データの増分更新、ワークフローの合理化、処理時間の短縮を行います。 意思決定を変革する、より迅速、スケーラブル、かつコスト効率の高いインサイトを得ます。 [ ハイパーキューブの使用に関するガイド ](../../query-service/hypercubes/overview.md) を参照して、高度な分析のロックを解除します。 |
| クエリエディターオブジェクトブラウザー | クエリエディターの新しいオブジェクトブラウザーを使用して、クエリの効率を高めます。 データセットをすばやく検索、フィルタリングおよびアクセスして、クエリの書き込みと絞り込みをすばやく行います。 リアルタイムのスキーマ更新とテーブルのインスタントメタデータを使用すると、ワークフローを合理化し、ナビゲーション時間を短縮し、クエリエクスペリエンスを強化できます。 データの可能性を解放し、分析を最適化します。 詳しくは、[ オブジェクトブラウザーの使用に関するガイド ](../../query-service/ui/user-guide.md#object-browser) を参照してください。 |
| 時間を計算 | スケジュール済みクエリの時間計算指標を新しく表示することで、リソース使用状況を制御できます。 クエリ実行レベルで計算時間を表示して、CTAS/ITAS バッチクエリのリソース使用を監視および最適化します。 各クエリ実行の開始時間、完了ステータスおよび計算時間を追跡します。 パフォーマンスを微調整し、コストを簡単に削減します。 クエリの効率を最大化する方法について詳しくは、[ 計算時間に関するガイド ](../../query-service/ui/query-schedules.md#compute-hours-at-job-level) を参照してください。 |

{style="table-layout:auto"}

クエリサービスについて詳しくは、[ クエリサービスの概要 ](../../query-service/home.md) を参照してください。

## セグメント化サービス {#segmentation-service}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| ストリーミングセグメント化条件の更新 | 2024 年 9 月のリリースより、ストリーミングセグメント化の対象となるオーディエンスの条件が更新されました。 これらの変更について詳しくは、[ ストリーミングセグメント化実施要件条件のアップデート ](../../segmentation/eligibility-criteria-update.md) を参照してください。 |
| 統合検索の実装 | セグメントビルダー内の検索動作で統合検索が使用されるようになりました。 これにより、オーディエンスを管理および検索する際に、より堅牢なエクスペリエンスが提供され、セグメントメンバーシップに再利用できます。 この変更について詳しくは、[ セグメントビルダーガイド ](../../segmentation/ui/segment-builder.md#rule-builder-canvas) を参照してください。 |

{style="table-layout:auto"}

[!DNL Segmentation Service] について詳しくは、[ セグメント化の概要 ](../../segmentation/home.md) を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

Experience Platform のソースを使用して、Adobe アプリケーションまたはサードパーティのデータソースからデータを取り込みます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!BADGE Beta]{type=Informative} UI での暗号化されたデータ取り込みのサポート | Experience Platformユーザーインターフェイスのソースワークスペースを使用して、クラウドストレージバッチソースから暗号化されたデータを取り込めるようになりました。 詳しくは、[UI での暗号化されたデータの取り込み ](../../sources/tutorials/ui/encryped-ingestion.md) に関するチュートリアルを参照してください。 |
| [!DNL Snowflake Streaming] ソースの一般提供 | [!DNL Snowflake Streaming] ソースは現在、一般提供（GA）中です。 このソースを使用して、[!DNL Snowflake] アカウントからExperience Platformにデータをストリーミングします。 詳しくは、[[!DNL Snowflake Streaming] overview](../../sources/connectors/databases/snowflake-streaming.md) を参照してください。 |
| [!DNL Google BigQuery] でのサービスアカウント認証のサポート | サービス アカウント認証を使用して [!DNL Google BigQuery] アカウントをExperience Platformに接続できるようになりました。 詳しくは、[[!DNL Google BigQuery]  概要 ](../../sources/connectors/databases/bigquery.md#generate-your-google-bigquery-credentials) を参照してください。<br> ![ スケジュール設定手順で「スケジュールとフォルダーを編集」オプションを強調表示したExperience Platformユーザーインターフェイスの画像。](../2024/assets/september/service_auth.png "Google BigQuery のサービス認証。"){width="250" align="center" zoomable="yes"} |
| サンプルデータのプレビューのスキップのサポート | 次のソースを使用したソース接続を作成する際に、データのプレビューをスキップするように選択できるようになりました。 <ul><li>[[!DNL Google BigQuery]](../../sources/tutorials/ui/create/databases/bigquery.md#skip-preview-of-sample-data)</li><li>[[!DNL Salesforce]](../../sources/tutorials/ui/create/crm/salesforce.md#skip-preview-of-sample-data)</li><li>[[!DNL Snowflake]](../../sources/tutorials/ui/create/databases/snowflake.md#skip-preview-of-sample-data)</li></ul> データのプレビューをスキップして、大きなバッチデータを取り込む際に発生する可能性のあるタイムアウトを回避できます。 計算フィールドと必須フィールドの自動検証が妨げられる可能性があります。 データのプレビューをスキップすることを選択した場合、マッピング中に計算フィールドと必須フィールドを手動で検証する必要がある可能性があります。 |
| [!DNL SFTP] でのチャンクの無効化のサポート | [!DNL SFTP] ソース内のチャンクを無効にできる設定を設定できるようになりました。 詳しくは、[[!DNL SFTP]  概要 ](../../sources/connectors/cloud-storage/sftp.md) を参照してください。 |

{style="table-layout:auto"}

詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
