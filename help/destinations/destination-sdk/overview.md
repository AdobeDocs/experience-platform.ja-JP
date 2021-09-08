---
description: Adobe Experience Platform Destination SDKは、選択したデータおよび認証形式に基づいて、オーディエンスとプロファイルのデータをエンドポイントに配信するためのExperience Platformの宛先統合パターンを設定できる設定APIのセットです。 設定はExperience Platformに保存され、APIを介して取得して追加の更新をおこなうことができます。
title: Adobe Experience Platform宛先SDK
exl-id: 7aca9f40-98c8-47c2-ba88-4308fc2b1798
source-git-commit: 2841adc0ce212a945c35ba38209d4c00c519ad7b
workflow-type: tm+mt
source-wordcount: '691'
ht-degree: 4%

---

# Adobe Experience Platform宛先SDK

## 概要 {#destinations-sdk}

Adobe Experience Platform Destination SDKは、選択したデータおよび認証形式に基づいて、オーディエンスとプロファイルのデータをエンドポイントに配信するためのExperience Platformの宛先統合パターンを設定できる設定APIのスイートです。 設定はExperience Platformに保存され、APIを介して取得して追加の更新をおこなうことができます。

宛先SDKのドキュメントでは、Adobe Experience Platform Destination SDKを使用して、Adobe Experience Platformとの製品化された宛先統合を設定、テスト、リリースし、成長し続ける宛先カタログに宛先を含める手順を説明しています。

![宛先カタログの概要](./assets/destinations-catalog-overview.png)

## 製品化されたカスタム統合 {#productized-custom-integrations}

宛先SDKパートナーは、製品化された宛先を[Experience Platformカタログ](/help/destinations/catalog/overview.md)に追加するメリットがあります。
1. 事前設定済みのパラメーターを使用して、顧客全体で統合設定を標準化し、顧客のセットアップエクスペリエンスをシンプル化します。
2. 顧客の設定と認識を簡素化するために、Experience Platformの宛先カタログにブランドの宛先カードを導入します。
3. Adobe Experience Platformおよびリアルタイム顧客データプラットフォームとの、製品化された宛先統合としてお勧めします。

Experience Platformをご利用のお客様は、アクティベーションのニーズに最適な、独自の非公開カスタムの宛先を作成できます。

![宛先SDKの視覚図](./assets/destination-sdk-visual.png)

<!--

## Types of destinations in Adobe Experience Platform {#types-of-destinations}

In Adobe Experience Platform, we distinguish between two destination types - *connections* and *extensions*. In the user interface, customers can choose between two types of connection destinations, Profile Export destinations and Segment Export destinations. For more details around the difference between the different destination types, read [Destination Types and Categories](https://experienceleague.adobe.com/docs/experience-platform/destinations/destination-types.html?lang=en).

![Destination types](./assets/types-of-destinations.png)

This documentation set provides you with all the necessary information to add your destination to Adobe Experience Platform, as a *connection*, either Profile Export or Segment Export. To set up an extension, visit the [Experience Platform Launch developer portal](https://developer.adobelaunch.com/extensions/).

-->

## サポートされる統合のタイプ {#supported-integration-types}

宛先SDKを通じて、Adobe Experience Platformは、REST APIエンドポイントを持つ宛先とのリアルタイム統合をサポートします。 Experience Platformとのリアルタイム統合は、次のような機能をサポートします。
* メッセージの変換と集計
* プロファイルのバックフィル
* オーディエンスの設定とデータ転送を初期化するための設定可能なメタデータ統合
* 設定可能な認証
* 宛先設定をテストおよび繰り返し実行するための一連のテストおよび検証API

「[統合の前提条件](./integration-prerequisites.md)」の記事の宛先側の技術要件を参照してください。


## 宛先SDKへのアクセス権の取得 {#get-access}

宛先SDKへのアクセスは、パートナーまたはExperience Platformのお客様の状況に応じて異なります。 詳しくは、以下の表を参照してください。


| パートナーまたは顧客のタイプ | 宛先SDKへのアクセス方法 |
---------|----------|
| 独立系ソフトウェアベンダー(ISV) | [Adobe交換プログラム](https://partners.adobe.com/exchangeprogram/experiencecloud.html)に参加し、宛先SDKへのアクセスをプロビジョニングしたExperience Platformサンドボックスを取得するリクエストを送信します。 |
| システムインテグレーター(SI) | [Adobeソリューションパートナープログラム](https://solutionpartners.adobe.com/home.html)でゴールドまたはプラチナのレベルにある必要があります。Experience Platformサンドボックスがプロビジョニングされ、Destination SDKにアクセスできます。 |
| Experience Platformのお客様（アクティベーションパッケージ） | デフォルトでは、Experience Platformサンドボックスと宛先SDKへのアクセス権を取得します。 |
| リアルタイムCDPパッケージでのExperience Platformのお客様 | 宛先SDKにアクセスできませんが、宛先SDKを使用して他の会社が設定し、Experience Platform組織全体に公開した、製品化されたすべての宛先にアクセスできます。 |

{style=&quot;table-layout:auto&quot;}

## 高レベルのプロセス {#process}

宛先を設定するプロセスを以下に示します。Experience Platformでの設定手順を示します。

1. ISVまたはSIの場合は、上記のセクションのアクセス情報を参照してください。 [Adobe Experience Platform ](https://helpx.adobe.com/legal/product-descriptions/adobe-experience-platform0.html) Activationのお客様は、この手順をスキップできます。
2. [宛先のオーサリング権限を有効にす](https://adobeexchangeec.zendesk.com/hc/en-us/articles/360037457812-Adobe-Experience-Platform-Sandbox-Accounts-Access-Adding-Users-and-Support) る、Experience Platformサンドボックスのプロビジョニングのリクエスト。
3. [製品ドキュメントに従](./configure-destination-instructions.md) って、統合を構築します。
4. [製品ドキュメ](./test-destination.md) ントに従って、統合をテストします。
5. [統合を送信し](./destination-publish-api.md) て、Adobeのレビューを受けます（標準応答時間は5営業日）。
6. ISVまたはSIが[製品化された統合](./overview.md#productized-custom-integrations)を作成している場合は、[セルフサービスのドキュメントプロセス](./docs-framework/documentation-instructions.md)を使用して、目的のExperience Leagueに関する製品ドキュメントページを作成します。
7. Adobeが承認すると、統合が[Experience Platformカタログ](/help/destinations/catalog/overview.md)に表示されます。
8. 統合を更新する場合は、同じ手順に従います。

## リファレンス {#reference}

Adobeは、次のドキュメントを読み、理解することをお勧めします。Experience Platform

* [Adobe Experience Platformの宛先の概要](https://experienceleague.adobe.com/docs/experience-platform/destinations/home.html?lang=en)
* [XDMスキーマ構成の基本](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/composition.html?lang=ja)
* [ID 名前空間の概要](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja)
