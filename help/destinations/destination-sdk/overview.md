---
description: Adobe Experience Platform Destination SDKは、選択したデータおよび認証形式に基づいて、オーディエンスとプロファイルのデータをエンドポイントに配信するためのExperience Platformの宛先統合パターンを設定できる設定 API のセットです。 設定は Experience Platform に保存され、API 経由で取得することで追加アップデートを入手できます。
title: Adobe Experience Platform Destination SDK
exl-id: 7aca9f40-98c8-47c2-ba88-4308fc2b1798
source-git-commit: 85b308b3f92a734fed0c885a574b71fa05684bb4
workflow-type: tm+mt
source-wordcount: '702'
ht-degree: 14%

---

# Adobe Experience Platform Destination SDK

## 概要 {#destinations-sdk}

Adobe Experience Platform Destination SDK は、Experience Platform の宛先統合パターンを設定し、選択したデータと認証形式に基づいて、オーディエンスとプロファイルのデータをエンドポイントに配信できるようにする設定 API のスイートです。設定は Experience Platform に保存され、API 経由で取得することで追加アップデートを入手できます。

Destination SDKドキュメントでは、Adobe Experience Platform Destination SDKを使用してAdobe Experience Platformとの製品化された宛先統合を設定、テスト、リリースし、宛先を増え続ける宛先カタログに含める手順を説明しています。

![宛先カタログの概要](./assets/destinations-catalog-overview.png)

## 製品化されたカスタム統合 {#productized-custom-integrations}

Destination SDKパートナーは、製品化された宛先を [Experience Platformカタログ](/help/destinations/catalog/overview.md):
1. 事前設定済みのパラメーターを使用して、顧客全体で統合設定を標準化し、顧客のセットアップエクスペリエンスを簡素化します。
2. 顧客の設定と認識を簡素化するために、Experience Platformの宛先カタログにブランド化した宛先カードを導入します。
3. Adobe Experience PlatformおよびReal-time Customer Data Platformとの製品化された宛先統合としてお勧めします。

Experience Platformのお客様は、アクティベーションのニーズに最適な、独自の非公開カスタムの宛先を作成できます。

![Destination SDKの視覚図](./assets/destination-sdk-visual.png)

<!--

## Types of destinations in Adobe Experience Platform {#types-of-destinations}

In Adobe Experience Platform, we distinguish between two destination types - *connections* and *extensions*. In the user interface, customers can choose between two types of connection destinations, Profile Export destinations and Segment Export destinations. For more details around the difference between the different destination types, read [Destination Types and Categories](https://experienceleague.adobe.com/docs/experience-platform/destinations/destination-types.html?lang=en).

![Destination types](./assets/types-of-destinations.png)

This documentation set provides you with all the necessary information to add your destination to Adobe Experience Platform, as a *connection*, either Profile Export or Segment Export. To set up an extension, visit the [Experience Platform Launch developer portal](https://developer.adobelaunch.com/extensions/).

-->

## サポートされる統合のタイプ {#supported-integration-types}

Destination SDKを通じて、Adobe Experience Platformは、REST API エンドポイントを持つ宛先とのリアルタイム統合をサポートします。 Experience Platformとのリアルタイム統合は、次のような機能をサポートします。
* メッセージの変換と集計
* プロファイルのバックフィル
* オーディエンスの設定とデータ転送を初期化するための設定可能なメタデータ統合
* 設定可能な認証
* 宛先設定をテストおよび繰り返すためのテストおよび検証 API のスイート

の宛先側の技術要件をお読みください [統合の前提条件](./integration-prerequisites.md) 記事。


## Destination SDK {#get-access}

Destination SDKへのアクセスは、パートナーまたはExperience Platformのお客様のステータスに応じて異なります。 詳しくは、以下の表を参照してください。


| パートナーまたは顧客のタイプ | Destination SDKへのアクセス方法 |
---------|----------|
| 独立系ソフトウェアベンダー (ISV) | 参加する [Adobe交換プログラム](https://partners.adobe.com/exchangeprogram/experiencecloud.html) Destination SDKへのアクセスをプロビジョニングするExperience Platformサンドボックスの取得をリクエストします。 |
| システムインテグレータ (SI) | Gold または Platinum のレベルである必要があります [Adobeソリューションパートナープログラム](https://solutionpartners.adobe.com/home.html)をプロビジョニングすると、Experience Platformサンドボックスがプロビジョニングされ、Destination SDKにアクセスできるようになります。 |
| Experience Platform顧客 [アクティベーションパッケージ](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-platform0.html) | デフォルトでは、Experience PlatformサンドボックスとDestination SDKにアクセスできます。 |
| Experience Platform顧客 [リアルタイム CDP パッケージ](https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform.html) | Destination SDKへのアクセス権はありませんが、Destination SDKを使用して他の会社が設定し、Experience Platform組織全体に公開した、製品化されたすべての宛先にアクセスできます。 |

{style=&quot;table-layout:auto&quot;}

## 高レベルのプロセス {#process}

宛先を設定するプロセスの概要を次に示します。Experience Platformを設定するプロセスを次に示します。

1. ISV または SI の場合は、上記のセクションの「アクセス情報の取得」を参照してください。 [Adobe Experience Platform Activation](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-platform0.html) のお客様は、この手順をスキップできます。
2. [Experience Platformサンドボックスのプロビジョニングのリクエスト](https://adobeexchangeec.zendesk.com/hc/en-us/articles/360037457812-Adobe-Experience-Platform-Sandbox-Accounts-Access-Adding-Users-and-Support) 宛先オーサリング権限を有効にします。
3. [統合の構築](./configure-destination-instructions.md) 製品ドキュメントに従います。
4. [統合のテスト](./test-destination.md) 製品ドキュメントに従います。
5. [統合の送信](./submit-destination.md) Adobeのレビュー（標準応答時間は 5 営業日）。
6. ISV または SI が [製品化統合](./overview.md#productized-custom-integrations)、 [セルフサービスドキュメント化プロセス](./docs-framework/documentation-instructions.md) をクリックして、宛先の製品ドキュメントページをExperience Leagueに作成します。
7. Adobeが承認すると、統合は [Experience Platformカタログ](/help/destinations/catalog/overview.md).
8. 統合を更新する場合は、同じ手順に従います。

## リファレンス {#reference}

Adobeでは、次のExperience Platformドキュメントを読み、理解することをお勧めします。

* [Adobe Experience Platform Destinations の概要](https://experienceleague.adobe.com/docs/experience-platform/destinations/home.html?lang=en)
* [XDM スキーマ構成の基本](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/composition.html?lang=ja)
* [ID 名前空間の概要](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja)
