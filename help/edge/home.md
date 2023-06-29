---
title: Adobe Experience Platform Web SDK の概要
description: Adobe Experience Platform Web SDK を使用して、Platform 機能を web サイトに統合する方法を説明します。
keywords: Adobe Experience Platform Web SDK;Platform Web SDK;Web SDK;エッジ;Visitor.js;AppMeasurement.js;AT.js;DIL.js;web sdk;SDK;web SDK;Launch;launch
exl-id: 1348144a-7d25-4c27-bc40-3daee2f043a6
source-git-commit: 12bd4c6c1993afc438b75a3e5163ebe2fe8a8dd0
workflow-type: tm+mt
source-wordcount: '803'
ht-degree: 100%

---

# Adobe Experience Platform Web SDK の概要 {#overview}

Adobe Experience Platform Web SDK は、Adobe Experience Cloud のお客様が Adobe Experience Platform Edge Network を通じて [!DNL Experience Cloud] の様々なサービスを操作できるようにする、クライアントサイド JavaScript ライブラリです。JavaScript ライブラリに加えて、Web SDK 設定に役立つ[タグ拡張](../tags/extensions/client/web-sdk/web-sdk-extension-configuration.md)があります。

タグを使用して Web SDK を設定し、ソリューションにデータを送信するためのステップごとのガイドについては、[Web SDK を使用した Adobe Experience Cloud の実装チュートリアル](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html?lang=ja)を参照してください。

>[!IMPORTANT]
>
>この製品は、より多くのユースケースに対応できるよう、常に進化を続け、成長しています。最新情報を入手し、現在サポートしているものを確認するには、[サポートされるユースケースページ](https://github.com/orgs/adobe/projects/18/views/1)を参照してください。

## Adobe Experience Edge

[!DNL Adobe Experience Platform Web SDK] は、[!DNL Adobe Experience Edge] を構成するコレクションの一部です。[!DNL Experience Edge] は、次のテクノロジーで構成されます。

* **[[!DNL Adobe Experience Platform Web SDK]](#overview)：**[!DNL Adobe] テクノロジーのデプロイを大幅に簡素化するための JavaScript SDK およびタグ拡張
* **[[!DNL Adobe Experience Platform Mobile SDK]](https://aep-sdks.gitbook.io/docs/getting-started/overview)：**&#x200B;お客様が新しいデプロイメント手法を使用できるようにするための v5 モバイル SDK の拡張機能
* **[[!DNL Adobe Experience Platform Edge Network]](../server-api/overview.md)：**[!DNL Adobe] 製品の新しいデプロイ手法を可能にする、サーバーのグローバル分散ネットワーク

[!DNL Adobe Experience Edge] は、すべてのアドレス可能なチャネルにわたる、低遅延のデータ収集、プラグ可能なコンピューティング、迅速なデータアクティブ化のための新しいフレームワークです。

[!DNL Adobe Experience Edge] は、すべてのチャネル（JavaScript、モバイル、サーバーサイド）に対して単一の統合 SDK を提供し、共通のアドビドメイン（`adobedc.net`）にデータを送信し、データとエクスペリエンスの配信のために単一のペイロードを返信します。

サーバーサイドでは、統一されたエッジゲートウェイと共通のプラットフォームサービスフレームワークにより、このリアルタイムコンピューティング環境に新しい機能を簡単にプラグインおよびデプロイできます。このアーキテクチャには次の特長があります。

* お客様の価値創出にかかる時間を短縮
* 「ポイント」統合の必要性をなくす
* 古いライブラリと比較してパフォーマンスを向上
* コストを削減
* イノベーションのスピードを上げる
* アドビのお客様に持続的な競争上の優位性を提供

単一の統合エッジシステムにより、お客様は、すべてのチャネルをまたいで、広告、マーケティングまたはパーソナライゼーションキャンペーンを統合されたエクスペリエンスとして管理できます。これにより、[!DNL Adobe] はお客様により低い総所有コストでサービスを提供できます。また、リアルタイムエッジをプラグ可能にして、[!DNL Adobe] とそのお客様がそのリアルタイムシステムに新しい機能およびお客様定義のロジックをより迅速に追加できるようにすることで、製品イノベーションのスピードを上げるのに役立ちます。

## ビデオの概要 {#video}

次のビデオでは、Adobe Experience Platform [!DNL Web SDK] および Adobe Experience Platform [!DNL Edge Network] の概要を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/34141?quality=12&learn=on)

## Web SDK に置き換わるライブラリ {#sdks}

Web SDK は、既存のライブラリの単なるラッパーではありません。既存のライブラリの機能を取り込んで新たに書き上げた、全く新しいライブラリです。その目的は、正しい順序で実行する必要があるタグ、ライブラリのバージョン管理との不整合、依存関係の管理などの課題を解決することです。[!DNL Experience Cloud] を実装するための新しい方法であり、[オープンソース](https://github.com/adobe/alloy)です。

Web SDK は、次の SDK を置き換えます。

* Visitor.js
* AppMeasurement.js
* AT.js
* DIL.js

新しいライブラリに加えて、アドビのソリューションに対する HTTP 要求を整理する新しいエンドポイントが追加されました。以前は、Visitor.js は訪問者 ID サービスに対してブロック呼び出しを送信した後、AT.js は Adobe Target に呼び出しを送信、DIL.js は Adobe Audience Manager に呼び出しを送信、最後に AppMeasurement.js は Adobe Analytics に呼び出しを送信していました。この新しいライブラリおよびエンドポイントでは、ID の取得、[!DNL Target] エクスペリエンスの取得、[!DNL Audience Manager] へのデータの送信、1 回の呼び出しでの Adobe Experience Platform へのデータの受け渡しが可能です。

次のビデオでは、Adobe Experience Platform [!DNL Web SDK] および Adobe Experience Platform [!DNL Edge Network] を実際に動作させています。ビデオの例では、アドビへの 1 回の呼び出しを使用して、[!DNL Experience Platform]、[!DNL Analytics]、[!DNL Audience Manager] および [!DNL Target] にデータを送信しています。

>[!VIDEO](https://video.tv.adobe.com/v/34148)

## 既存のライブラリから Web SDK への移行 {#migrating-to-web-sdk}

[既存のライブラリ](#sdks)から Web SDK への移行を簡素化するため、アドビでは、合理化されたアップグレードパスを提供し、web サイト全体を一度に移行する必要なく、web サイトのページごとに Web SDK に移行できます。

つまり、あるページで Web SDK を使用し、他のページには既存のライブラリを残して、移行できるようになります。

### at.js から Web SDK への移行に関する考慮事項 {#considerations}

[!DNL at.js] を使用するページを Web SDK に移行する前に、次の Web SDK 設定オプションを必ず有効にします。これにより、[!DNL at.js] を含むページから Web SDK を使用するページに移動する間、訪問者プロファイルが保持されます。

* [`idMigrationEnabled`](fundamentals/configuring-the-sdk.md#id-migration-enabled)
* [`targetMigrationEnabled`](fundamentals/configuring-the-sdk.md#targetMigrationEnabled)


>[!IMPORTANT]
>
>次の Target 機能は、at.js から Web SDK に移行する際にはサポートされません。
> * [リダイレクトオファー](https://experienceleague.adobe.com/docs/target/using/experiences/offers/offer-redirect.html?lang=ja)
> * [CNAME とクロスドメインサポート](https://developer.adobe.com/target/implement/client-side/atjs/atjs-cookies/?lang=jp)

at.js から Web SDK に移行した後、設定から `targetMigrationEnabled` オプションを削除する必要があります。



