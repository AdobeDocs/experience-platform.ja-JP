---
title: Experience Platformのプレリリースノート
description: Adobe Experience Platformの最新のリリースノートのプレビュー。
hide: true
hidefromtoc: true
exl-id: f2c41dc8-9255-4570-b459-4f9fc28ee58b
source-git-commit: a26ad18b1e44b3198db9e8a36ad3749ed8a0afa2
workflow-type: tm+mt
source-wordcount: '1116'
ht-degree: 18%

---

# Adobe Experience Platformのプレリリースノート

>[!IMPORTANT]
>
>このドキュメントは、今月のリリースノートの **プレビュー** として提供することを目的としています。 リリース項目は変更される場合があり、最終リリースで追加または削除される場合があります。

>[!TIP]
>
>その他のAdobe Experience Platform アプリケーションのリリースノートについては、次のドキュメントを参照してください。
>
>- [Adobe Journey Optimizer](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/whats-new/release-notes)
>- [Adobe Journey Optimizer B2B](https://experienceleague.adobe.com/ja/docs/journey-optimizer-b2b/user/release-notes)
>- [Customer Journey Analytics](https://experienceleague.adobe.com/en/docs/analytics-platform/using/releases/pre-release-notes)
>- [連合オーディエンス構成](https://experienceleague.adobe.com/en/docs/federated-audience-composition/using/e-release-notes)
>- [Real-Time CDP Collaboration](https://experienceleague.adobe.com/en/docs/real-time-cdp-collaboration/using/latest)

**リリース日：2025 年 8 月**

Adobe Experience Platformの既存の機能に対する新機能とアップデート：

- [アラート](#alerts)
- [宛先](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [セグメント化サービス](#segmentation-service)
- [ソース](#sources)

## アラート {#alerts}

Experience Platformでは、様々なExperience Platform アクティビティに関するイベントベースのアラートを登録できます。 Experience Platform ユーザーインターフェイスの「[!UICONTROL &#x200B; アラート &#x200B;]」タブを使用して、様々なアラートルールを購読し、UI 内またはメール通知を通じてアラートメッセージを受け取るように選択できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| ストリーミングスループット容量アラート | 3 つの新しいアラートにより、ユーザーはアラートを登録および設定して、ストリーミングスループット容量のパフォーマンスをプロアクティブに管理および監視できます。 新しいアラートには、ストリーミングのスループットが 80%、90% に達した場合、処理能力の制限を超えた場合などが含まれます。 詳細については、「[capacity alert rules](../observability/alerts/rules.md#capacity) guide」を参照してください。 |

アラートについて詳しくは、[[!DNL Observability Insights]  概要 ](../observability/home.md) を参照してください。

## 宛先 {#destinations}

Experience Platformから [!DNL Destinations] データの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。 宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新しい宛先**

| 宛先 | 説明 |
| --- | --- |
| [!DNL Acxiom Real ID Audience] の宛先 | [!DNL Acxiom Real ID Audience Connection] の宛先を使用すると、[!DNL Acxiom's]Real ID™[ テクノロジー ](https://www.acxiom.com/real-id/real-id/) 使用してオーディエンスを強化し、[!DNL Altice]、[!DNL Ampersand]、[!DNL Comcast] などの複数のプラットフォームに対してオーディエンスをアクティブ化できます。 |


**更新された宛先**

| 宛先 | 説明 |
| --- | --- |
| [!DNL LinkedIn] 宛先の認証有効期限の詳細 | 期限切れの資格情報について二度と心配する必要はありません。 アカウントの有効期限に関する情報がExperience Platform インターフェイスに直接表示されるようになり、[!DNL LinkedIn] 認証の有効期限が切れるタイミングを確認し、データフローが中断される前に更新できるようになりました。 |
| [!DNL Data Landing Zone] 宛先の暗号化のサポート | 書き出したデータを暗号化で保護します。 RSA 形式の公開鍵を添付して、書き出したファイルを暗号化できるようになりました。これにより、他のクラウドストレージの宛先が機密情報に提供するのと同じレベルのセキュリティが提供されます。 |
| [[!DNL Microsoft Bing]](../destinations/catalog/advertising/bing.md) 内部アップグレード | 2025 年 8 月 11 日（PT）以降、宛先カタログに 2 つの **[!DNL Microsoft Bing]** カードが並んで表示されるようになります。 これは、宛先サービスの内部アップグレードが原因です。 既存の **[!DNL Microsoft Bing]** 宛先コネクタの名前は、**[!UICONTROL （非推奨）Microsoft Bing]** に変更され、**[!UICONTROL Microsoft Bing]** という名前の新しいカードが使用できるようになりました。 新しいアクティブ化データ フローには、カタログ内の新しい **[!UICONTROL Microsoft Bing]** 接続を使用します。 **[!UICONTROL （非推奨）のMicrosoft Bing]** の宛先へのアクティブなデータフローがある場合、自動的に更新されるので、ユーザー側で対応する必要はありません。 <br><br>[Flow Service API](https://developer.adobe.com/experience-platform-apis/references/destinations/) を使用してデータフローを作成する場合は、[!DNL flow spec ID] を更新し、次の値に [!DNL connection spec ID] す必要があります。<ul><li>フロー仕様 ID: `8d42c81d-9ba7-4534-9bf6-cf7c64fbd12e`</li><li>接続仕様 ID: `dd69fc59-3bc5-451e-8ec2-1e74a670afd4`</li></ul> このアップグレードの後、**へのデータフローで、** アクティブ化されたプロファイルの数が減少 [!DNL Microsoft Bing] する場合があります。 このドロップは、この宛先プラットフォームへのすべてのアクティベーションに対して **ECID マッピング要件** が導入されたことによって発生します。 |
| 宛先の追加 [!DNL Amazon Ads] 識別子 | Amazon広告の宛先で新しい ID （`firstName`、`lastName`、`street`、`city`、`state`、`zip`、`country`）がサポートされるようになりました。 これらのフィールドは、オーディエンスの一致率の向上を目的としており、オプションの SHA256 ハッシュを含むプレーンテキストで渡されます。 |
| [!DNL Marketo] 宛先カードの統合 | 統合された宛先カードを使用すると、[!DNL Marketo] しい宛先設定を簡素化できます。 [!DNL Marketo] V2 と V3 のカードを 1 つの合理化されたオプションに統合し、適切な宛先を選択してすばやく開始できるようになりました。 |

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 2024 年 11 月以前に作成されたデータフローのデータセット書き出しスケジュールを拡張します | 2024 年 11 月より前に作成されたデータセット書き出しデータフローがある組織の場合、これらのデータフローは 2025 年 9 月 1 日に機能しなくなります。 2025 年 9 月 1 日（PT）以降もデータの書き出しを維持するためにデータフローが必要な場合、[ このガイド ](../destinations/ui/dataset-expiration-update.md) の手順に従って、データセットを書き出す各宛先のスケジュールを拡張する必要があります。 |
| 宛先の検索、フィルタリングおよびタグ付け機能の強化 | 「参照」タブと「アカウント」タブの検索機能、フィルタリング機能、タグ付け機能の強化により、宛先管理ワークフローが向上します。 名前で特定のデータフローとアカウントを検索し、宛先プラットフォーム、ステータス、日付などの様々な条件でフィルタリングし、カスタムタグを作成して宛先を整理できるようになりました。 列の並べ替えは、前回のデータフロー実行時などのキーフィールドでも使用できるので、宛先接続の識別と管理が容易になります。 |

詳しくは、[ 宛先の概要 ](../destinations/home.md) を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Experience Platformに取り込むデータの共通の構造と定義（スキーマ）を提供するオープンソース仕様です。 XDM 標準規格に準拠しているので、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客アクションから有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| モデルベースのスキーマ | モデルベースのスキーマを使用して、データモデリングを簡略化できます。 包括的なハウツー例やガイダンスを使用して、スキーマをより簡単に作成できるようになりました。 この機能は、現在、Campaign Orchestration のライセンスホルダーが使用でき、GA の Data Distillerのお客様にも拡大される予定で、データモデリングがよりアクセスしやすく効率的になります。 |

詳しくは、[XDM の概要 ](../xdm/home.md) を参照してください。

## セグメント化サービス {#segmentation-service}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。オーディエンスは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて設定できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| オーディエンス見積り | オーディエンスの推定がセグメントビルダー内で自動的に生成されるようになりました。 この値は、オーディエンスを変更するたびに更新され、常に最新のオーディエンスルールを反映します。 |

詳しくは、[[!DNL Segmentation Service] 概要](../segmentation/home.md)を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!BADGE Beta]{type=Informative}UI での Azure プライベートリンクのサポート | プライベートネットワーク接続でデータを保護します。 プライベートエンドポイントを作成し、パブリックインターネットをバイパスするデータフローを設定して、機密データのセキュリティとネットワーク分離を強化できるようになりました。 |
| [!DNL Marketo] ソースドキュメントのアップデート | [!DNL Marketo] データがExperience Platformに入った際にどのように変換されるかを完全に把握できます。 すべてのフィールドマッピングにデータ変換の詳細な説明が含まれるようになりました。これにより、`PersonID` ータがどのように変 `leadID` し、`eventType` がどのように変 `activityType` するかを正確に理解できます。 |
| [!DNL Azure Blob Storage] のサービスプリンシパル認証のサポート | サービスプリンシパル認証を使用して、[!DNL Azure Blob Storage] アカウントをExperience Platformに接続できるようになりました。 |

詳しくは、[ソースの概要](../sources/home.md)を参照してください。

<!--

## Query Service {#query-service}

Adobe Experience Platform Query Service provides a robust SQL interface for data analysis and exploration across the platform.

**New or updated features**

| Feature | Description |
| ------- | ----------- |
| Data Distiller Session Management | Take control of your data analysis sessions with enhanced session management. You can now monitor and manage your sessions more effectively across development and production environments, giving you better visibility into your query performance and resource usage. |

For more information, read the [Query Service overview](../query-service/home.md).

## B2B CDP {#b2b-cdp}

Real-Time CDP B2B Edition provides comprehensive B2B customer data management capabilities, enabling organizations to build unified customer profiles, create sophisticated B2B audiences, and activate data across various marketing channels.

**New or updated features**

| Feature | Description |
| ------- | ----------- |
| Lookup Support for B2B Classes Only | Streamline your B2B data access with focused lookup support. You can now look up Person (Profile), Experience Events, Account, and Opportunity entities directly through the Entities API. This simplified approach helps you access the most important B2B data more efficiently while reducing complexity. |
| B2B Namespace and Schema Updates | Experience a cleaner, more streamlined B2B data model. We've simplified the B2B namespace and schema structure by removing complex relationship mappings and non-primary identity support for certain B2B classes. This makes your B2B data easier to work with and understand. |

For more information, read the [Real-Time CDP B2B Edition overview](../rtcdp/b2b-overview.md).

-->
