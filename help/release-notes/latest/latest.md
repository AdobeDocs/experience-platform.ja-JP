---
title: Adobe Experience Platform リリースノート 2025年5月
description: Adobe Experience Platform の 2025年5月のリリースノート。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: 3f143c34d31483e91e176b1c69d82db38a8bc6b5
workflow-type: tm+mt
source-wordcount: '1333'
ht-degree: 24%

---

# Adobe Experience Platform リリースノート

>[!TIP]
>
>その他のAdobe Experience Platform アプリケーションのリリースノートについては、次のドキュメントを参照してください。
>
>- [Adobe Journey Optimizer](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/whats-new/release-notes)
>- [Adobe Journey Optimizer B2B](https://experienceleague.adobe.com/en/docs/journey-optimizer-b2b/user/release-notes)
>- [Customer Journey Analytics](https://experienceleague.adobe.com/ja/docs/analytics-platform/using/releases/latest)
>- [連合オーディエンス構成](https://experienceleague.adobe.com/en/docs/federated-audience-composition/using/release-notes)
>- [Real-Time CDP Collaboration](https://experienceleague.adobe.com/en/docs/real-time-cdp-collaboration/using/latest)

**リリース日：2025年5月20日（PT）**

Adobe Experience Platformの既存の機能およびドキュメントのアップデート：

- [カタログサービス](#catalog-service)
- [データ準備](#data-prep)
- [宛先](#destinations)
- [ID サービス](#identity)
- [クエリサービス](#query-service)
- [サンドボックス](#sandboxes)
- [セグメント化サービス](#segmentation-service)
- [ソース](#sources)

## カタログサービス {#catalog-service}

カタログサービスは、Adobe Experience Platform 内のデータの場所と系列のレコードのシステムです。Experience Platformに取り込まれるすべてのデータはファイルおよびディレクトリとして Data Lake に保存されますが、カタログには、参照や監視のために、これらのファイルおよびディレクトリのメタデータと説明が保持されます。

| 機能 | 説明 |
| --- | --- |
| データセットレベルの保持ルールによるデータストレージの最適化 | 指定した期間に基づいて古いデータを削除する保存ポリシーにより、データ・ストレージを効率的に管理します。 <ul><li>**データセットの保持**：データレイクおよびプロファイルストアから古いデータを削除するデータセットルールを定義します。</li><li>**ストレージインサイト**：インライン指標を使用して使用状況を監視し、ライセンス使用権限へのコンプライアンスを確保します。</li><li>**表示の強化**：監視を強化して、取得から削除までのデータセットアクティビティを追跡します。</li><li>**管理の効率化**：アクセス保持設定、監視、監査ログ、インサイトを 1 つの統合されたビューで表示します。</li></ul> 詳しくは、[ データセット保持ルール ](../../catalog/datasets/user-guide.md#data-retention-policy) に関するガイドを参照してください。 |

{style="table-layout:auto"}

詳しくは、[ カタログサービスの概要 ](../../catalog/home.md) を参照してください。

## データ準備 {#data-prep}

データ準備を使用して、エクスペリエンスデータモデル（XDM）との間でデータのマッピング、変換、検証を行う。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| Adobe Analyticsのインポートおよびエクスポートマッピングのサポート | Adobe Analytics ソースコネクタを使用する際に、Adobe Analytics レポートスイートデータのインポートおよびエクスポートマッピング機能を使用できるようになりました。 <br><br> マッピングを CSV ファイルに書き出し、スプレッドシートでローカルに設定します。 その後、UI のマッピングインターフェイスを使用して、更新したマッピングをExperience Platformに読み込むことができます。 この機能を使用すると、UI で手動で作成しなくても、多数のマッピングを設定できます。 さらに、新しいデータフローを作成する際に、マッピングのコピーをExperience Platformに直接アップロードすると、ワークフローを高速化できます。 <br><br> 詳しくは、[Adobe AnalyticsのExperience Platformへの接続 ](../../sources/tutorials/ui/create/adobe-applications/analytics.md) に関するガイドを参照してください。</br> |

{style="table-layout:auto"}

詳しくは、[ データ準備の概要 ](../../data-prep/home.md) を参照してください。

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| --- | --- |
| [Facebook カスタムオーディエンス ](../../destinations/catalog/social/facebook.md) アドレス関連の識別子のアップグレードとサポート | 2025 年 5 月 23 日（PT）以降、2025 年 6 月を通じて、一時的に、宛先カタログに 2 つの **[!DNL Facebook Custom Audience]** の宛先カードが数時間まで表示される場合があります。 これは、宛先サービスの内部アップグレードと、ターゲティングの改善や Facebook プロパティでのプロファイルとのマッチングのための新しいフィールドのサポートが原因です。 新しいアドレス関連フィールドについて詳しくは、[ サポートされる ID](#supported-identities) の節を参照してください。 <br><br> 「**[!UICONTROL （新規） Facebook カスタムオーディエンス]**」というラベルの付いたカードが表示された場合は、このカードを新しいアクティベーションデータフローに使用します。 既存のデータフローは自動的に更新されるので、ユーザー側で対応する必要はありません。 この期間中に既存のデータフローに加えた変更は、アップグレード後も保持されます。 アップグレードが完了すると、「**[!UICONTROL （新規） Facebook カスタムオーディエンス]**」宛先カードの名前が **[!DNL Facebook Custom Audience]** に変更されます。 <br><br>[Flow Service API](https://developer.adobe.com/experience-platform-apis/references/destinations/) を使用してデータフローを作成する場合は、[!DNL flow spec ID] を更新し、次の値に [!DNL connection spec ID] す必要があります。 <ul><li>フロー仕様 ID: `bb181d00-58d7-41ba-9c15-9689fdc831d3`</li><li>接続仕様 ID: `c8b97383-2d65-4b7a-9913-db0fbfc71727`</li></ul> |
| [Google カスタマーマッチ ](../../destinations/catalog/advertising/google-customer-match.md) のその他の識別子サポート | Google カスタマーマッチの宛先で、アドレス関連フィールドのマッピングがサポートされるようになり、Google Platform のマッチ率が向上しました。 <br><br>Googleがアドレスと一致するようにするには、4 つのアドレスフィールド（`address_info_first_name`、`address_info_last_name`、`address_info_country_code` および `address_info_postal_code`）をすべてマッピングし、書き出されたプロファイルでこれらのフィールドに欠けているデータがないことを確認する必要があります。 <br> いずれかのフィールドがマッピングされていないか、データが欠落している場合、Googleはアドレスと一致しません。 |
| [Facebook](../../destinations/catalog/social/facebook.md) 接続用のアカウントの有効期限の列 | 「参照」タブと「[ アカウント ](../../destinations/ui/destinations-workspace.md#accounts) タブに、Facebook アカウントトークンの有効期限 [ が表示される ](../../destinations/ui/destinations-workspace.md#browse) うになりました。 |
| API で作成されたデータセットの書き出し | API で作成されたデータセットを書き出せるようになりました。 UI で作成されたデータセットのみが書き出し可能であった以前の制限は解除されました。 詳しくは、[ データセットの書き出し ](../../destinations/ui/export-datasets.md) を参照してください。 |

{style="table-layout:auto"}

詳しくは、[ 宛先の概要 ](../../destinations/home.md) を参照してください。

## ID サービス {#identity}

Adobe Experience Platform ID サービスを使用すると、デバイスやシステム間で ID を紐付けることで、顧客とその行動の全体像を把握し、インパクトのある、個人的なデジタルエクスペリエンスをリアルタイムで提供できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!DNL Identity Graph Linking Rules] | [!DNL Identity Graph Linking Rules] が一般公開されました。 「グラフの折りたたみ」 [!DNL Identity Graph Linking Rules] 防ぎ、Experience Platformとアプリケーションをまたいでパーソナライズされたマーケティングのために、明確で正確な顧客プロファイルを確保します。 主な機能は次のとおりです。<ul><li>[ グラフシミュレーションツール ](../../identity-service/identity-graph-linking-rules/graph-simulation.md)：アルゴリズムを探索し、ID 設定の設定をテストします。</li><li> [ID 設定 ](../../identity-service/identity-graph-linking-rules/identity-settings-ui.md)：一意の名前空間を設定し、優先度を設定する。</li><li>[ID ダッシュボード ](../../identity-service/identity-graph-linking-rules/implementation-guide.md#validate-your-graphs)：グラフを監視し、ID 設定を検証します。</li></ul> **注意**:ID 設定を手動で設定するまで、データは変更されません。 |

{style="table-layout:auto"}

詳しくは、[ID サービスの概要 ](../../identity-service/home.md) を参照してください。

## クエリサービス {#query-service}

クエリサービスを使用した標準 SQL を使用して、Adobe Experience Platform Data Lake でデータをクエリします。 データセットをシームレスに組み合わせ、クエリ結果から新しいデータセットを生成してレポートを強化したり、データサイエンスワークフローを有効にしたり、リアルタイム顧客プロファイルへの取り込みを容易にしたりします。 例えば、顧客トランザクションデータを行動データと結合して、ターゲットマーケティングキャンペーンの高価値オーディエンスを特定できます。

**新機能または更新された機能**

| 機能 | 説明 |
|--------|-------------|
| JWT から OAuth への資格情報の移行 | サービスの中断を避けるために、有効期限のない JWT 資格情報は、2025 年 6 月 30 日（PT）までに OAuth サーバー間に移行される必要があります。 この変更により、セキュリティとプラットフォームの一貫性が向上します。 詳しくは、[JWT から OAuth サーバー間資格情報への移行ガイド ](../../query-service/ui/migrate-jwt-to-oauth.md) を参照してください。 |
| 従来の結果テーブル（限定提供） | ドラッグして選択するワークフローに依存するユーザーは、ブラウザーネイティブの選択とコピー動作をサポートする従来の結果テーブルへのアクセスをリクエストできます。 貼り付けた出力はタブ区切りなので、Excel では列の位置が維持され、読みやすくなるので、スプレッドシートのレビューや QA のプロセスが容易になります。 詳しくは、[ クエリエディター UI ガイド ](../../query-service/ui/user-guide.md#legacy-results-table) を参照してください。 |

[!DNL Query Service] について詳しくは、[[!DNL Query Service]  の概要](../../query-service/home.md)を参照してください。

## サンドボックス {#sandboxes}

Adobe Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。このニーズに対応するために、Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つサンドボックスが用意されています。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| サンドボックスツールプラグインサポートの拡張 | チャネル設定、統合決定、実験設定/バリアントなど、サンドボックスツールを使用して、追加の依存オブジェクトを含むキャンペーンを移行できるようになりました。 サポートされているすべてのキャンペーンオブジェクトとサポートされているすべてのAdobe Journey Optimizer オブジェクトのリストについては、[ サンドボックスツール ](../../sandboxes/ui/sandbox-tooling.md#adobe-journey-optimizer-objects) ガイドを参照してください。 |

{style="table-layout:auto"}

サンドボックスについて詳しくは、[ サンドボックスの概要 ](../../sandboxes/home.md) を参照してください。

## セグメント化サービス {#segmentation-service}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。オーディエンスは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて設定できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| ストリーミングセグメント化条件の更新 | 2025 年 5 月のリリースより、オーディエンスがストリーミングセグメント化の対象となる条件が更新されました。 これらの変更について詳しくは、[ ストリーミングセグメント化実施要件条件のアップデート ](../../segmentation/eligibility-criteria-update.md) を参照してください。 |

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

Experience Platform のソースを使用して、Adobe アプリケーションまたはサードパーティのデータソースからデータを取り込みます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!DNL MySQL] の基本認証のサポート | これで、基本認証を使用して [!DNL MySQL] データベースをExperience Platformに接続できます。 詳しくは、[[!DNL MySQL]  ソースの概要 ](../../sources/connectors/databases/mysql.md) を参照してください。 |

{style="table-layout:auto"}

詳しくは、[ソースの概要](../../sources/home.md)を参照してください。