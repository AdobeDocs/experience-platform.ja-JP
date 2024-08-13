---
title: Adobe Experience Platform Web Software Development Kit （SDK）の概要
description: Adobe Experience Platform Web SDK を使用して、Platform 機能を web サイトに統合する方法を説明します。
source-git-commit: ac8bb272a8af26f90a866541d513e50bf2dfa8b0
workflow-type: tm+mt
source-wordcount: '796'
ht-degree: 28%

---


# Adobe Experience Platform Web SDK {#overview}

>[!IMPORTANT]
>
>2024 年 4 月末に、Adobe Experience Platform Web SDK は、Internet Explorer のすべてのバージョンのサポートを削除します。

Adobe Experience Platform Web Software Development Kit （SDK）は、Adobe Experience Cloudのお客様がAdobe Experience Platform Edge Networkを通じてそのサービスを操作できるようにする、クライアントサイド JavaScript ライブラリです。

Adobeでは、Web SDK を実装するために次の 2 つの方法を提供しています。

* [Web SDK タグ拡張機能 ](../tags/extensions/client/web-sdk/web-sdk-extension-configuration.md)。 詳しくは、[Web SDK を使用したAdobe Experience Cloudの実装 ](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html?lang=ja) 方法に関するチュートリアルを参照してください。
* Web SDK JavaScript ライブラリを使用した手動実装。

このユーザーガイドには、Web SDK JavaScript ライブラリとタグ拡張機能（該当する場合）の両方を使用してExperience Cloudソリューションを操作する手順が含まれています。

## Experience PlatformEdge Network {#edge-network}

Experience Platform Web SDK は、Adobe Experience Platform Edge Networkを構成するツールのコレクションの一部です。

Edge Networkは、次のコンポーネントで構成されます。

* **[Experience PlatformWeb SDK](#overview):** Adobeテクノロジの展開を簡単にするJavaScript ライブラリおよびタグ拡張。
* **[Experience PlatformMobile SDK](https://developer.adobe.com/client-sdks/home/):** 新しいデプロイメント手法を使用できる v5 モバイル SDK の拡張機能。
* **[Edge Networkサーバー API](../server-api/overview.md):** 様々なデータ収集、パーソナライゼーション、広告、マーケティングのユースケースに使用できるサーバーサイド API。 Server API は、サーバー、IoT デバイス、セットトップボックス、その他の様々なデバイスで使用できます。

このEdge Networkは、すべてのアドレス可能なチャネルにわたる、低遅延のデータ収集、プラグ可能なコンピューティング、迅速なデータアクティブ化のためのフレームワークです。 各チャネル（web、モバイル、サーバーサイド）に対して 1 つの統合 SDK を提供し、共通のAdobeドメイン（`adobedc.net`）にデータを送信し、データとエクスペリエンスの配信のために 1 つのペイロードを返信します。

サーバーサイドでは、統合エッジゲートウェイと共通のプラットフォームサービスフレームワークにより、このリアルタイムコンピューティング環境に新しい機能を簡単にデプロイできます。 このアーキテクチャには次の特長があります。

* お客様の価値創出にかかる時間を短縮
* 「ポイント」統合の必要性をなくす
* 古いライブラリと比較してパフォーマンスを向上
* コストを削減
* イノベーションのスピードを上げる
* アドビのお客様に持続的な競争上の優位性を提供

単一の統合エッジシステムにより、すべてのチャネルをまたいで、広告、マーケティングまたはパーソナライゼーションキャンペーンを統合されたエクスペリエンスとして管理できます。 また、Adobeはお客様により低い総所有コストでサービスを提供できます。 エッジシステムは、ほとんどのタイプのデータに対応するように設計されているので、独自のデータモデルをマッピングして、複数のExperience Cloud製品で取り込むことができます。

## ビデオの概要 {#video}

Adobe Experience Platform [!DNL Web SDK] と [!DNL Edge Network] の概要については、以下のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/34141?quality=12&learn=on)

## Web SDK に置き換わるライブラリ {#sdks}

Web SDK は、既存のライブラリの単なるラッパーではありません。既存のライブラリの機能を取り入れて新たに書き上げた新しいライブラリです。 その目的は、正しい順序で実行する必要があるタグ、ライブラリのバージョン管理との不整合、依存関係の管理などの課題を解決することです。[!DNL Experience Cloud] を実装するための新しい方法であり、[オープンソース](https://github.com/adobe/alloy)です。

Web SDK は、次の SDK を置き換えます。

* `Visitor.js`
* `AppMeasurement.js`
* `AT.js`
* `DIL.js`

新しいライブラリに加えて、アドビのソリューションに対する HTTP 要求を整理する新しいエンドポイントが追加されました。以前は、`Visitor.js` は訪問者 ID サービスにブロック呼び出しを送信し、次にAdobe Targetに呼び出しを送信し、`AT.js` にAdobe Audience Managerに呼び出しを送信 `DIL.js`、最後にAdobe Analyticsに呼び出しを送信しまし `AppMeasurement.js`。 この新しいライブラリおよびエンドポイントでは、ID の取得、[!DNL Target] エクスペリエンスの取得、[!DNL Audience Manager] へのデータの送信、1 回の呼び出しでの Adobe Experience Platform へのデータの受け渡しが可能です。

次のビデオでは、Adobe Experience Platform [!DNL Web SDK] および Adobe Experience Platform [!DNL Edge Network] を実際に使用して説明しています。ビデオの例では、アドビへの 1 回の呼び出しを使用して、[!DNL Experience Platform]、[!DNL Analytics]、[!DNL Audience Manager] および [!DNL Target] にデータを送信しています。

>[!VIDEO](https://video.tv.adobe.com/v/34148)

## 既存のライブラリから Web SDK への移行 {#migrating-to-web-sdk}

[ 既存のライブラリ ](#sdks) から Web SDK への移行を簡素化するために、Adobeでは合理化されたアップグレードパスを提供します。 このパスを使用すると、web サイト全体を一度に移行する必要なく、web サイトの各ページを Web SDK に移行できます。 既存のライブラリを他のページに配置しながら、特定のページで Web SDK を使用できます。 準備が整ったら、その他のページも移行できます。

### `AT.js` から Web SDK への移行に関する考慮事項 {#considerations}

`AT.js` を使用するページを Web SDK に移行する前に、次の Web SDK 設定オプションを必ず有効にします。これらのオプションにより、`AT.js` を含むページから Web SDK を使用するページに移動する間、訪問者プロファイルが保持されます。

* [`idMigrationEnabled`](/help/web-sdk/commands/configure/idmigrationenabled.md)
* [`targetMigrationEnabled`](/help/web-sdk/commands/configure/targetmigrationenabled.md)


>[!IMPORTANT]
>
>次の Target 機能は、at.js から Web SDK に移行する際にはサポートされません。
>
>* [リダイレクトオファー](https://experienceleague.adobe.com/docs/target/using/experiences/offers/offer-redirect.html?lang=ja)
>* [CNAME とクロスドメインサポート](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/atjs-cookies.html)

`AT.js` から Web SDK に移行した後、設定から `targetMigrationEnabled` オプションを削除します。
