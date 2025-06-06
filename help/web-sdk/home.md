---
title: Adobe Experience Platform Web Software Development Kit （SDK）の概要
description: Adobe Experience Platform Web SDKを使用して、Experience Platform機能を web サイトに統合する方法を説明します。
exl-id: 1348144a-7d25-4c27-bc40-3daee2f043a6
source-git-commit: 7f3459f678c74ead1d733304702309522dd0018b
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 5%

---

# Adobe Experience Platform Web SDK {#overview}

Adobe Experience Platform Web SDKは、Adobe Experience Cloudのお客様がAdobe Experience Platform Edge Networkを通じてサービスを操作できるようにする、クライアントサイド JavaScript ライブラリです。

Web SDKは次の 2 つの方法で実装できます。

* [Web SDK タグ拡張機能 ](../tags/extensions/client/web-sdk/web-sdk-extension-configuration.md)。 詳しくは、[Web SDKを使用したAdobe Experience Cloudの実装 ](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html?lang=ja) 方法に関するチュートリアルを参照してください。
* [Web SDK JavaScript ライブラリ ](install/library.md) を使用した手動実装。

このガイドには、Web SDK JavaScript ライブラリとタグ拡張機能の両方を使用してExperience Cloud ソリューションを操作する手順が含まれています。

## Experience PlatformEdge Network {#edge-network}



Experience Platform Web SDKは、Adobe Experience Platform Edge Networkの一部で、以下が含まれます。

* **[Experience Platform Web SDK](#overview)**: Adobe テクノロジーのデプロイメントをシンプル化するためのJavaScript ライブラリおよびタグ拡張機能です。
* **[Experience Platform Mobile SDK](https://developer.adobe.com/client-sdks/home/)**：新しいデプロイメント手法のための v5 モバイルSDKの拡張機能。
* **[Edge Network API](https://developer.adobe.com/data-collection-apis/docs/api/)**: データ収集、パーソナライゼーション、広告およびマーケティングのユースケース用のサーバーサイド API。 サーバー、IoT デバイス、セットトップボックス、その他のデバイスで使用できます。

Edge Networkは、すべてのアドレス可能なチャネルにわたって、低遅延のデータ収集、プラグ可能なコンピューティング、迅速なデータアクティブ化を提供します。 Web、モバイルおよびサーバーサイドチャネル用の統合SDKを 1 つ提供し、データを共通のAdobe ドメイン（`adobedc.net`）に送信して、データとエクスペリエンスの配信に使用する 1 つのペイロードを受け取ります。

サーバーサイドでは、統合エッジゲートウェイと共通のプラットフォームサービスフレームワークにより、新しい機能のデプロイを簡素化できると同時に、次のメリットがあります。

* お客様の価値創出までの時間の短縮
* 「ポイント」統合の必要性を排除
* 古いライブラリと比較してパフォーマンスを向上させる。
* 運用コストの削減
* イノベーションのスピードの向上
* Adobeのお客様に持続的な競争上の優位性を提供する。

統合されたエッジシステムにより、あらゆるチャネルをまたいで、広告、マーケティングおよびパーソナライゼーションキャンペーンを管理できます。 総所有コストを削減し、様々なデータタイプをサポートするので、複数のExperience Cloud製品で使用するデータモデルをマッピングできます。

## ビデオの概要 {#video}

Adobe Experience Platform [!DNL Web SDK] と [!DNL Edge Network] の概要については、以下のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/37263?quality=12&learn=on&captions=jpn)

## Web SDK に置き換わるライブラリ {#sdks}

Web SDKは、既存のライブラリの機能を統合するためにゼロから構築された新しいオープンソースライブラリです。 タグの実行順序、バージョンの不整合、依存関係の管理に関する問題に対処し、[!DNL Experience Cloud] ールを実装するための新しい [ オープンソース ](https://github.com/adobe/alloy) 方法を提供します。

Web SDKは、次のものに代わるものです。

* `Visitor.js`
* `AppMeasurement.js`
* `AT.js`
* `DIL.js`

また、Adobe ソリューションへの HTTP リクエストを効率化する新しいエンドポイントも導入されます。 以前は、`Visitor.js`、`AT.js`、`DIL.js` および `AppMeasurement.js` に対して複数の呼び出しが必要でした。 1 回の呼び出しで ID を取得し、[!DNL Target] エクスペリエンスを取得し、[!DNL Audience Manager] にデータを送信し、Adobe Experience Platformにデータを渡すことができるようになりました。

以下のビデオを視聴すると、1 回の呼び出しで [!DNL Experience Platform]、[!DNL Analytics]、[!DNL Audience Manager] および [!DNL Target] にデータを送信する、Adobe Experience Platform [!DNL Web SDK] および [!DNL Edge Network] の動作を確認できます。

>[!VIDEO](https://video.tv.adobe.com/v/3413667?captions=jpn)

## 既存のライブラリから Web SDK への移行 {#migrating-to-web-sdk}

Adobeは、合理化されたアップグレードパスを提供し、任意の [ 既存のライブラリ ](#sdks) から Web SDKへの移行を簡素化します。 一度にサイト全体を移行しなくても、web サイトの各ページを個別に移行できます。 一部のページでは Web SDKを使用できますが、既存のライブラリは別のページで使用することができ、段階的に移行することができます。

### `AT.js` から Web SDKへの移行に関する考慮事項 {#considerations}

`AT.js` を使用してページを Web SDKに移行する前に、次の Web SDK設定オプションを有効にして、ページ間を移動する際に訪問者プロファイルが維持されるようにします。

* [`idMigrationEnabled`](/help/web-sdk/commands/configure/idmigrationenabled.md)
* [`targetMigrationEnabled`](/help/web-sdk/commands/configure/targetmigrationenabled.md)

>[!IMPORTANT]
>
>次の Target 機能は、`at.js` から Web SDKに移行する際にはサポートされません。
>
>* [リダイレクトオファー](https://experienceleague.adobe.com/docs/target/using/experiences/offers/offer-redirect.html?lang=ja)
>* [CNAME とクロスドメインサポート](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/atjs-cookies.html?lang=ja)

`AT.js` から Web SDKに移行した後、設定から「`targetMigrationEnabled`」オプションを削除します。
