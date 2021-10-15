---
description: Adobe Experience Platform Destination SDK は、選択したデータおよび認証形式に基づいて、Experience Platformがオーディエンスとプロファイルのデータをエンドポイントに配信するための宛先統合パターンを設定できる一連の設定 API です。 設定は Experience Platform に保存され、API 経由で取得することで追加アップデートを入手できます。
title: Adobe Experience Platform宛先 SDK
exl-id: 7aca9f40-98c8-47c2-ba88-4308fc2b1798
source-git-commit: bd65cfa557fb42d23022578b98bc5482e8bd50b1
workflow-type: tm+mt
source-wordcount: '702'
ht-degree: 14%

---

# Adobe Experience Platform宛先 SDK

## 概要 {#destinations-sdk}

Adobe Experience Platform Destination SDK は、Experience Platform の宛先統合パターンを設定し、選択したデータと認証形式に基づいて、オーディエンスとプロファイルのデータをエンドポイントに配信できるようにする設定 API のスイートです。設定は Experience Platform に保存され、API 経由で取得することで追加アップデートを入手できます。

宛先 SDK のドキュメントでは、Adobe Experience Platform Destination SDK を使用して、Adobe Experience Platformとの製品化された宛先統合を設定、テスト、リリースし、その宛先を増え続ける宛先カタログの一部にする手順を説明しています。

![宛先カタログの概要](./assets/destinations-catalog-overview.png)

## 製品化されたカスタム統合 {#productized-custom-integrations}

宛先 SDK パートナーは、製品化された宛先を [Experience Platformカタログ ](/help/destinations/catalog/overview.md) に追加するメリットがあります。
1. 事前設定済みのパラメーターを使用して、顧客全体で統合設定を標準化し、顧客の設定エクスペリエンスをシンプル化します。
2. 顧客の設定と認識を簡素化するために、Experience Platformの宛先カタログにブランドの宛先カードを導入します。
3. Adobe Experience PlatformとReal-time Customer Data Platformの製品化された宛先統合としてお勧めします。

Experience Platformをご利用のお客様は、独自の非公開のカスタムの宛先を作成できます。この宛先は、アクティベーションのニーズに最適です。

![宛先 SDK の視覚図](./assets/destination-sdk-visual.png)

<!--

## Types of destinations in Adobe Experience Platform {#types-of-destinations}

In Adobe Experience Platform, we distinguish between two destination types - *connections* and *extensions*. In the user interface, customers can choose between two types of connection destinations, Profile Export destinations and Segment Export destinations. For more details around the difference between the different destination types, read [Destination Types and Categories](https://experienceleague.adobe.com/docs/experience-platform/destinations/destination-types.html?lang=en).

![Destination types](./assets/types-of-destinations.png)

This documentation set provides you with all the necessary information to add your destination to Adobe Experience Platform, as a *connection*, either Profile Export or Segment Export. To set up an extension, visit the [Experience Platform Launch developer portal](https://developer.adobelaunch.com/extensions/).

-->

## サポートされる統合のタイプ {#supported-integration-types}

宛先 SDK を通じて、Adobe Experience Platformは、REST API エンドポイントを持つ宛先とのリアルタイムの統合をサポートします。 Experience Platformとのリアルタイム統合は、次のような機能をサポートします。
* メッセージの変換と集計
* プロファイルのバックフィル
* オーディエンスの設定とデータ転送を初期化するための設定可能なメタデータ統合
* 設定可能な認証
* 宛先設定をテストおよび繰り返し実行するための一連のテストおよび検証 API

「[ 統合の前提条件 ](./integration-prerequisites.md)」の記事で、宛先側の技術要件を確認してください。


## 宛先 SDK へのアクセス権の取得 {#get-access}

宛先 SDK へのアクセス権は、パートナーまたはExperience Platformのお客様の状況に応じて異なります。 詳しくは、以下の表を参照してください。


| パートナーまたは顧客のタイプ | 宛先 SDK へのアクセス方法 |
---------|----------|
| 独立系ソフトウェアベンダー (ISV) | [Adobe交換プログラム ](https://partners.adobe.com/exchangeprogram/experiencecloud.html) に参加し、宛先 SDK へのアクセスをプロビジョニングしたExperience Platformサンドボックスを取得するようリクエストします。 |
| システムインテグレーター (SI) | [Adobeソリューションパートナープログラム ](https://solutionpartners.adobe.com/home.html) でゴールドレベルまたはプラチナレベルになっている必要があり、Experience Platformサンドボックスがプロビジョニングされ、宛先 SDK にアクセスできます。 |
| [ アクティベーションパッケージ ](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-platform0.html) のExperience Platformのお客様 | デフォルトでは、Experience Platformサンドボックスと宛先 SDK にアクセスできます。 |
| [ リアルタイム CDP パッケージ ](https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform.html) のExperience Platformのお客様 | 宛先 SDK にアクセスできませんが、宛先 SDK を使用して他の会社が設定し、Experience Platform組織全体に公開した、製品化されたすべての宛先にアクセスできます。 |

{style=&quot;table-layout:auto&quot;}

## 高レベルのプロセス {#process}

宛先を設定するプロセスを次に示します。Experience Platformは次のとおりです。

1. ISV または SI の場合は、上記のセクションのアクセス情報を参照してください。 [Adobe Experience Platform ](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-platform0.html) Activation のお客様は、この手順をスキップできます。
2. [宛先サンドボックスをプロビジョニ](https://adobeexchangeec.zendesk.com/hc/en-us/articles/360037457812-Adobe-Experience-Platform-Sandbox-Accounts-Access-Adding-Users-and-Support) ングし、宛先のオーサリング権限を有効にするリクエスト。
3. [製品ドキュメント](./configure-destination-instructions.md) に従って、統合を構築します。
4. [製品ドキュメント](./test-destination.md) に従って、統合をテストします。
5. [統合を送](./destination-publish-api.md) 信して、Adobeのレビューを受けます（標準応答時間は 5 営業日）。
6. ISV または SI が [ 製品化された統合 ](./overview.md#productized-custom-integrations) を作成している場合は、[ セルフサービスのドキュメントプロセス ](./docs-framework/documentation-instructions.md) を使用して、目的のExperience Leagueに関する製品ドキュメントページを作成します。
7. Adobeが承認すると、統合は [Experience Platformカタログ ](/help/destinations/catalog/overview.md) に表示されます。
8. 統合を更新する場合は、同じ手順に従います。

## リファレンス {#reference}

Adobeでは、次のドキュメントを読み、理解することをお勧めしますExperience Platform。

* [Adobe Experience Platformの宛先の概要](https://experienceleague.adobe.com/docs/experience-platform/destinations/home.html?lang=en)
* [XDM スキーマ構成の基本](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/composition.html?lang=ja)
* [ID 名前空間の概要](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja)
