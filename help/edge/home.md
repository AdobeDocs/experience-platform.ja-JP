---
title: Adobe Experience Platform Web ソフトウェア開発キット (SDK) の概要
description: Adobe Experience Platform Web SDK を使用して、Platform 機能を Web サイトに統合する方法について説明します。
exl-id: 1348144a-7d25-4c27-bc40-3daee2f043a6
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '776'
ht-degree: 33%

---

# Adobe Experience Platform Web SDK の概要 {#overview}

Adobe Experience Platform Web Software Development Kit(SDK) は、Adobe Experience Cloudのお客様がAdobe Experience Platform Edge Network を通じてサービスを操作できるようにする、クライアントサイド JavaScript ライブラリです。 Adobeには、Web SDK を実装する 2 つの方法があります。

* を使用した手動実装 `alloy.js`. このユーザーガイドは、この実装方法に関するドキュメントを提供します。
* The [Web SDK タグ拡張機能](../tags/extensions/client/web-sdk/web-sdk-extension-configuration.md). 詳しくは、 [Web SDK を使用したAdobe Experience Cloudの実装のチュートリアル](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html?lang=ja) を参照してください。

## Experience Platformエッジネットワーク

Experience PlatformWeb SDK は、Adobe Experience Platform Edge Network を構成するツールのコレクションの一部です。 Edge ネットワークは、次のコンポーネントで構成されています。

* **[Experience PlatformWeb SDK](#overview):** Adobeテクノロジーの導入を大幅に簡略化する JavaScript SDK およびタグ拡張。
* **[Experience Platformモバイル SDK](https://developer.adobe.com/client-sdks/documentation/):** v5 モバイル SDK の拡張機能で、顧客が新しいデプロイメント手法を使用できるようになりました。
* **[Experience Platformエッジネットワークサーバー API](../server-api/overview.md):** 様々なデータ収集、パーソナライゼーション、広告、マーケティングの使用例に使用できる API です。 Server API は、サーバ、IoT デバイス、セットトップボックス、その他の様々なデバイスで使用できます。

Edge Network は、低レイテンシのデータ収集、プラグ可能なコンピューティング、およびアドレス可能なすべてのチャネルにわたる迅速なデータアクティベーションを実現するためのフレームワークです。 チャネル（JavaScript、モバイル、サーバー側）ごとに 1 つの統合 SDK を提供し、共通のAdobeドメイン (`adobedc.net`) に送信され、データおよびエクスペリエンス配信用の単一のペイロードを受け取ります。

サーバー側では、統合エッジゲートウェイと共通のプラットフォームサービスフレームワークを使用して、新しい機能をこのリアルタイムコンピューティング環境に簡単に導入できます。 このアーキテクチャには次の特長があります。

* お客様の価値創出にかかる時間を短縮
* 「ポイント」統合の必要性をなくす
* 古いライブラリと比較してパフォーマンスを向上
* コストを削減
* イノベーションのスピードを上げる
* アドビのお客様に持続的な競争上の優位性を提供

単一の統合エッジシステムを使用すると、顧客は、統合エクスペリエンスとして、すべてのチャネルで広告、マーケティングまたはパーソナライゼーションキャンペーンを管理できます。 また、Adobeは、TCO（総所有コスト）の低いサービスをお客様に提供できます。 エッジシステムは、ほとんどのタイプのデータに対応するように設計されており、複数のExperience Cloud製品で取り込む独自のデータモデルをマッピングできます。

## ビデオの概要 {#video}

次のビデオでは、Adobe Experience Platform [!DNL Web SDK] および Adobe Experience Platform [!DNL Edge Network] の概要を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/34141?quality=12&learn=on)

## Web SDK に置き換わるライブラリ {#sdks}

Web SDK は、既存のライブラリの単なるラッパーではありません。これは新しいライブラリで、既存のライブラリの機能を組み込むために一から書かれています。 その目的は、正しい順序で実行する必要があるタグ、ライブラリのバージョン管理との不整合、依存関係の管理などの課題を解決することです。[!DNL Experience Cloud] を実装するための新しい方法であり、[オープンソース](https://github.com/adobe/alloy)です。

Web SDK は、次の SDK を置き換えます。

* `Visitor.js`
* `AppMeasurement.js`
* `AT.js`
* `DIL.js`

新しいライブラリに加えて、アドビのソリューションに対する HTTP 要求を整理する新しいエンドポイントが追加されました。前、 `Visitor.js` 次に、ブロック呼び出しを訪問者 ID サービスに送信し、 `AT.js` Adobe Targetに呼び出しを送り `DIL.js` がAdobe Audience Managerに呼び出しを送り、最後に `AppMeasurement.js` はAdobe Analyticsに呼び出しを送信しました。 この新しいライブラリおよびエンドポイントでは、ID の取得、[!DNL Target] エクスペリエンスの取得、[!DNL Audience Manager] へのデータの送信、1 回の呼び出しでの Adobe Experience Platform へのデータの受け渡しが可能です。

次のビデオでは、Adobe Experience Platform [!DNL Web SDK] および Adobe Experience Platform [!DNL Edge Network] を実際に使用して説明しています。ビデオの例では、アドビへの 1 回の呼び出しを使用して、[!DNL Experience Platform]、[!DNL Analytics]、[!DNL Audience Manager] および [!DNL Target] にデータを送信しています。

>[!VIDEO](https://video.tv.adobe.com/v/34148)

## 既存のライブラリから Web SDK への移行 {#migrating-to-web-sdk}

次のいずれかから簡単に移行できます。 [既存のライブラリ](#sdks) Web SDK へのアップグレード時に、Adobeは合理化されたアップグレードパスを提供します。 このパスを使用すると、Web サイト全体を一度に移行する必要なく、Web サイトの個々のページを Web SDK に移行できます。 既存のライブラリは他のページに存在するのに対し、特定のページで Web SDK を使用できます。 準備が整ったら、他のページも移行できます。

### の移行 `AT.js` Web SDK に関する考慮事項 {#considerations}

`AT.js` を使用するページを Web SDK に移行する前に、次の Web SDK 設定オプションを必ず有効にします。これらのオプションにより、 `AT.js` を Web SDK を使用してページに追加します。

* [`idMigrationEnabled`](fundamentals/configuring-the-sdk.md#id-migration-enabled)
* [`targetMigrationEnabled`](fundamentals/configuring-the-sdk.md#targetMigrationEnabled)


>[!IMPORTANT]
>
>次の Target 機能は、at.js から Web SDK に移行する際にはサポートされません。
>
>* [リダイレクトオファー](https://experienceleague.adobe.com/docs/target/using/experiences/offers/offer-redirect.html?lang=ja)
>* [CNAME とクロスドメインサポート](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/atjs-cookies.html)

移行後 `AT.js` を Web SDK に追加し、 `targetMigrationEnabled` 」オプションを選択します。
