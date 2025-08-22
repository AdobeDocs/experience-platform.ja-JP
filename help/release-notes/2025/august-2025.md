---
title: Adobe Experience Platform リリースノート 2025年8月
description: Adobe Experience Platform の 2025年8月のリリースノート。
exl-id: d93e98f3-d165-4710-ad1d-2ad3857cd0f8
source-git-commit: d2b605925a8fd7ea06f198ba8a9f85747a2e585b
workflow-type: tm+mt
source-wordcount: '1643'
ht-degree: 20%

---

# Adobe Experience Platform リリースノート

>[!TIP]
>
>その他のAdobe Experience Platform アプリケーションのリリースノートについては、次のドキュメントを参照してください。
>
>- [Adobe Journey Optimizer](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/whats-new/release-notes)
>- [Adobe Journey Optimizer B2B](https://experienceleague.adobe.com/ja/docs/journey-optimizer-b2b/user/release-notes)
>- [Customer Journey Analytics](https://experienceleague.adobe.com/ja/docs/analytics-platform/using/releases/pre-release-notes)
>- [連合オーディエンス構成](https://experienceleague.adobe.com/ja/docs/federated-audience-composition/using/e-release-notes)
>- [Real-Time CDP Collaboration](https://experienceleague.adobe.com/ja/docs/real-time-cdp-collaboration/using/latest)

**リリース日：2025年8月19日（PT）**


Adobe Experience Platformの既存の機能に対する新機能とアップデート：

- [アラート](#alerts)
- [カタログサービス](#catalog-service)
- [宛先](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [リアルタイム顧客プロファイル](#profile)
- [サンドボックス](#sandboxes)
- [セグメント化サービス](#segmentation-service)
- [ソース](#sources)

## アラート {#alerts}

Experience Platformでは、様々なExperience Platform アクティビティに関するイベントベースのアラートを登録できます。 Experience Platform ユーザーインターフェイスの「[!UICONTROL &#x200B; アラート &#x200B;]」タブを使用して、様々なアラートルールを購読し、UI 内またはメール通知を通じてアラートメッセージを受け取るように選択できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| ストリーミングスループット容量アラート | 3 つの新しいアラートにより、ユーザーはアラートを登録および設定して、ストリーミングスループット容量のパフォーマンスをプロアクティブに管理および監視できます。 新しいアラートには、ストリーミングのスループットが 80%、90% に達した場合、処理能力の制限を超えた場合などが含まれます。 詳細については、「[capacity alert rules](../../observability/alerts/rules.md#capacity) guide」を参照してください。 |

アラートについて詳しくは、[[!DNL Observability Insights]  概要 ](../../observability/home.md) を参照してください。

## カタログサービス {#catalog-service}

カタログサービスは、Adobe Experience Platform 内のデータの場所と系列のレコードのシステムです。Experience Platformに取り込まれるすべてのデータはファイルおよびディレクトリとして Data Lake に保存されますが、カタログには、参照や監視のために、これらのファイルおよびディレクトリのメタデータと説明が保持されます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| リアルタイム顧客プロファイルのデータ保持 | リアルタイム顧客プロファイルのデータ保持期間は **30 日に 1 回** 更新のみ）できます。 |

カタログサービスについて詳しくは、[ カタログサービスの概要 ](../../catalog/home.md) を参照してください。

## 宛先 {#destinations}

Experience Platformから [!DNL Destinations] データの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。 宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

>[!IMPORTANT]
>
>**データセット書き出しスケジュール拡張機能**
>
>2024 年 11 月より前に作成されたデータセット書き出しデータフローがある組織の場合、これらのデータフローは **2025 年 9 月 1 日** に機能しなくなります。 2025 年 9 月 1 日（PT）以降もデータの書き出しを維持するためにデータフローが必要な場合、[ このガイド ](../../destinations/ui/dataset-expiration-update.md) の手順に従って、データセットを書き出す各宛先のスケジュールを拡張する必要があります。

>[!IMPORTANT]
>
>**API ベースの宛先に必要な IP許可リストの更新**
>
>ストリーミング宛先の書き出しエンジンのアップグレードに伴い、API ベースの宛先に [IP](../../destinations/catalog/streaming/ip-address-allow-list.md)許可リストを使用する組織は、次の IP アドレスを許可リストに追加する必要があります **2025 年 9 月 15 日（PT）より前**。
>
>**必須 IP アドレス：**
>
>```
>3.209.222.108
>3.211.230.204
>35.169.227.49
>66.117.18.133
>66.117.18.134
>66.117.18.135
>```
>
>**この変更は、次の宛先タイプに適用されます。**
>
>- [ ストリーミングオーディエンス書き出し宛先 ](../../destinations/destination-types.md#streaming-destinations) （[Pega CDH リアルタイムオーディエンス ](/help/destinations/catalog/personalization/pega-v2.md)、[Salesforce Marketing Cloud](../../destinations/catalog/email-marketing/salesforce-marketing-cloud-exact-target.md) および [Oracle Eloqua](../../destinations/catalog/email-marketing/oracle-eloqua-api.md) との API ベースの統合）
>- [Destination SDK](../../destinations/destination-sdk/getting-started.md) 経由で作成されたパブリックまたはプライベートの宛先
>
>**対処が必要：** Adobeと連携して API ベースのストリーミング宛先に IP アドレスを許可リストに加える許可リストに加えるした場合、上記の IP アドレスを宛先に追加して、API ベースの宛先へのデータフローが中断されないようにする必要があります。

**新しい宛先**

| 宛先 | 説明 |
| --- | --- |
| [[!DNL Acxiom Real ID Audience Connection]](../../destinations/catalog/advertising/acxiom-real-id-audience-connection.md) の宛先 | [!DNL Acxiom Real ID Audience Connection] の宛先を使用すると、[!DNL Acxiom's] 実 ID[ テクノロジー ](https://www.acxiom.com/real-id/real-id/) 使用してオーディエンスを強化し、[!DNL Altice]、[!DNL Ampersand]、[!DNL Comcast] などの複数のプラットフォームに対してオーディエンスをアクティブ化できます。 |
| Enhanced [[!DNL Marketo Engage]](../../destinations/catalog/adobe/marketo-engage-connection.md) の宛先 | 拡張 [[!DNL Marketo Engage]](../../destinations/catalog/adobe/marketo-engage-connection.md) 宛先は、既存の [[!DNL (Legacy) (V2) Marketo Engage]](../../destinations/catalog/adobe/marketo-engage.md) コネクタのアップグレードバージョンです。 この新しいコネクタは、従来のコネクタの既存のオーディエンス同期機能に加えて、プロファイル同期機能を提供し、[!DNL Marketo Engage] とのより緊密な統合を実現します。 <br> [[!DNL (Legacy) (V2) Marketo Engage]](../../destinations/catalog/adobe/marketo-engage.md) コネクタは、**2026 年 3 月** に非推奨（廃止予定）になります。 新しい **[[!UICONTROL 0&rbrace;Marketo Engage&rbrace; の宛先へのスムーズな移行を確実に行うには、次のポイントと必要な操作を確認します。]](../../destinations/catalog/adobe/marketo-engage-connection.md)** <ul><li>既存の **[!UICONTROL （従来の）（V2）Marketo Engage]** のすべてのユーザーは、2026 年 3 月までに新しい **[!UICONTROL Marketo Engage]** の宛先に移行する必要があります。</li><li> **既存のデータフローは、自動的には移行されません。** 新しい [3&rbrace;Marketo Engage&rbrace; の宛先への ](../../destinations/ui/connect-destination.md) 新しい接続を設定 **[!UICONTROL し、そこでオーディエンスをアクティブ化する必要があります。]**</li></ul> |

**更新された宛先**

| 宛先 | 説明 |
| --- | --- |
| [[!DNL Microsoft Bing]](../../destinations/catalog/advertising/bing.md) 内部アップグレード | 2025 年 8 月 11 日（PT）以降、短期間のうちに、宛先カタログに 2 つの **[!DNL Microsoft Bing]** カードが並んで表示される場合があります。 これは、宛先サービスの内部アップグレードが原因です。 既存の **[!DNL Microsoft Bing]** 宛先コネクタの名前は、**[!UICONTROL （非推奨）Microsoft Bing]** に変更され、**[!UICONTROL Microsoft Bing]** という名前の新しいカードが使用できるようになりました。 <br> アップグレードが完了し、非推奨カードが宛先カタログから削除されました。 新しいアクティブ化データ フローについては、カタログ内の **[!UICONTROL Microsoft Bing]** 接続を使用します。 **[!UICONTROL （非推奨）のMicrosoft Bing]** の宛先へのアクティブなデータフローがある場合、自動的に更新されるので、ユーザー側で対応する必要はありません。 <br><br>[Flow Service API](https://developer.adobe.com/experience-platform-apis/references/destinations/) を使用してデータフローを作成する場合は、[!DNL flow spec ID] を更新し、次の値に [!DNL connection spec ID] す必要があります。<ul><li>フロー仕様 ID: `8d42c81d-9ba7-4534-9bf6-cf7c64fbd12e`</li><li>接続仕様 ID: `dd69fc59-3bc5-451e-8ec2-1e74a670afd4`</li></ul> このアップグレードの後、**へのデータフローで、** アクティブ化されたプロファイルの数が減少 [!DNL Microsoft Bing] する場合があります。 このドロップは、この宛先プラットフォームへのすべてのアクティベーションに対して **ECID マッピング要件** が導入されたことによって発生します。 |
| [[!DNL LinkedIn]](../../destinations/catalog/social/linkedin.md) および [LinkedIn でマッチしたオーディエンス ](../../destinations/catalog/social/linkedin-b2b.md) 宛先の認証有効期限の詳細 | [!DNL LinkedIn] の宛先の認証の有効期限に関する情報がExperience Platform インターフェイスに直接表示されるようになり、認証の有効期限が切れるタイミングを確認し、データフローが中断される前に更新できるようになりました。 トークンの有効期限は、「**[!UICONTROL アカウント]**&#x200B;**[[!UICONTROL または]](../../destinations/ui/destinations-workspace.md#accounts)** 参照 **[[!UICONTROL タブの「アカウントの有効期限]](../../destinations/ui/destinations-workspace.md#browse)** 列から監視できます。 |

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 宛先の検索、フィルタリングおよびタグ付け機能の強化 | 「参照 [ タブと ](../../destinations/ui/destinations-workspace.md#browse) アカウント [ タブの検索機能、フィルタリング機能、タグ付け機能の強化により、宛先管理ワークフロー ](../../destinations/ui/destinations-workspace.md#accounts) 改善します。 <br> 特定のデータフローとアカウントを名前で検索し、宛先プラットフォーム、ステータス、日付などの様々な条件でフィルタリングして、カスタムタグを作成して宛先を整理できるようになりました。 列の並べ替えは、前回のデータフロー実行時などのキーフィールドでも使用できるので、宛先接続の識別と管理が容易になります。<br> ![ 「参照」タブでの宛先データフロー検索のアニメーションデモ ](../../destinations/assets/ui/workspace/search.gif) |

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Experience Platformに取り込むデータの共通の構造と定義（スキーマ）を提供するオープンソース仕様です。 XDM 標準規格に準拠しているので、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客アクションから有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| モデルベースのスキーマ | モデルベースのスキーマを使用して、データモデリングを簡略化できます。 包括的なハウツー例やガイダンスを使用して、スキーマをより簡単に作成できるようになりました。 この機能は、現在、Campaign Orchestration のライセンスホルダーが使用でき、GA の Data Distillerのお客様にも拡大される予定で、データモデリングがよりアクセスしやすく効率的になります。 |

詳しくは、[XDM の概要 ](../../xdm/home.md) を参照してください。

## リアルタイム顧客プロファイル {#profile}

リアルタイム顧客プロファイルは、すべてのチャネルからのデータを 1 つのプロファイルに統合することで、各顧客のアクションにつながる統一されたビューを提供します。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| Entities API のルックアップ機能の強化 | Entities API で以下がサポートされるようになりました。 <ul><li>人物（プロファイル）</li><li>エクスペリエンスイベント</li><li>アカウント</li><li>Opportunity</li></ul> このアップデートにより、API の使用が簡素化され、最適なパフォーマンスと信頼性が確保されます。 以前に結合テーブルやカスタムのマルチエンティティタイプなど、他のエンティティタイプの検索を使用していた場合、今では API の使用状況を確認し、改善されたエクスペリエンスを活用する優れた機会となっています。 詳しくは、[Real-Time CDB B2B edition アーキテクチャアップグレードガイド ](../../rtcdp/b2b-architecture-upgrade.md) を参照してください。 |

リアルタイム顧客プロファイルについて詳しくは、[ プロファイルの概要 ](../../profile/home.md) を参照してください。

## サンドボックス {#sandboxes}

Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| インポート ワークフローでの依存関係オブジェクトの重複排除 | オブジェクトの拡散を避けるために、同じ名前のオブジェクトが検出された場合、サンドボックスツールは常に既存のオブジェクトを再利用するようになりました。 この変更は、次のオブジェクトに適用されます。 <ul><li>スキーマ</li><li>フィールドグループ</li><li>オーディエンス</li><li>`decisioning_ranking`</li><li>`decisioning_rules`</li></ul> 詳しくは、[ サンドボックスツールでサポートされるオブジェクトに関するガイド ](../../sandboxes/ui/sandbox-tooling.md#objects-supported-for-sandbox-tooling) を参照してください。 |
| 組織をまたいだパッケージ共有のサンドボックス全体のサポート | サンドボックスツールで、組織パッケージ共有全体で **サンドボックス全体** タイプインがサポートされるようになりました。 組織全体でサンドボックスパッケージとマルチオブジェクトパッケージの両方を共有できるようになりました。 詳しくは、[ サンドボックスツールでサポートされるオブジェクトに関するガイド ](../../sandboxes/ui/sharing-packages-across-orgs.md) を参照してください。 |

サンドボックスについて詳しくは、[ サンドボックスの概要 ](../../sandboxes/home.md) を参照してください。

## セグメント化サービス {#segmentation-service}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。オーディエンスは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて設定できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| オーディエンス見積り | オーディエンスの推定がセグメントビルダー内で自動的に生成されるようになりました。 この値は、オーディエンスを変更するたびに更新され、常に最新のオーディエンスルールを反映します。 さらに、見積もりは、サンプリングデータの信頼区間に基づく **範囲** として表示されるようになりました。 |

詳しくは、[[!DNL Segmentation Service] 概要](../../segmentation/home.md)を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!BADGE Beta]{type=Informative}UI での [!DNL Azure Private Links] のサポート | UI で選択したソースグループに対して [!DNL Azure Private Links] を使用できるようになりました。 この機能を使用して、ソースが接続できるプライベートエンドポイントを作成します。 プライベートエンドポイントを使用すると、パブリックインターネットをバイパスする接続とデータフローを設定して、機密データのセキュリティとネットワーク分離を強化できます。 [!DNL Azure Private Links] のサポートは、次のソースで利用できます。 <ul><li>[[!DNL Azure Blob Storage]](../../sources/connectors/cloud-storage/blob.md)</li><li>[[!DNL ADLS Gen2]](../../sources/connectors/cloud-storage/adls-gen2.md)</li><li>[[!DNL Azure File Storage]](../../sources/connectors/cloud-storage/azure-file-storage.md)</li><li>[[!DNL Snowflake]](../../sources/connectors/databases/snowflake.md)</li></ul> 詳しくは、[[!DNL Azure Private Links]](../../sources/tutorials/ui/private-link.md) のガイドを参照してください。 |
| [!DNL Azure Blob Storage] の認証の強化 | サービスプリンシパルベースの認証を使用して、[!DNL Azure Blob Storage] ソースをExperience Platformに接続できるようになりました。 サービスプリンシパルベースの認証を使用すると、セキュリティの強化、資格情報のローテーションの容易さ、アカウントへのきめ細かいアクセス制御が可能になります。 詳しくは、[[!DNL Azure Blob Storage] 概要](../../sources/connectors/cloud-storage/blob.md)を参照してください。 |

詳しくは、[ソースの概要](../../sources/home.md)を参照してください。

<!--
| [!DNL Marketo] source documentation updates | Get complete visibility into how your [!DNL Marketo] data is transformed when it enters Experience Platform. All field mappings now include detailed explanations of data transformations, so you can understand exactly how your `PersonID` becomes `leadID` and `eventType` becomes `activityType`. |
-->
