---
solution: Experience Platform
title: データ収集の概要
description: Adobe Experience Platformにデータを送信する方法を説明します。
exl-id: 03ce5339-e68d-4adf-8c3c-82846a626dad
source-git-commit: 3d51f01d314587510d900d335dc92fedb8ac31e8
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 2%

---

# データ収集の概要

Adobe Experience Platformは、様々なソースからカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Networkに送信するための一連のテクノロジーを提供します。 その後、そのデータをエンリッチメントしたり、変換したり、AdobeまたはAdobe以外の宛先に配信したりできます。

Adobeは、データ収集用の専用ライブラリで次のコード言語をサポートしています。

* **JavaScript**: Web サイトおよび Web ベース アプリケーション用
* **Kotlin**:Androidデバイス用
* **Swift**:iOS デバイスの場合
* **Brightscript**:Roku デバイスの場合
* **Flutter**:Flutter を使用したAndroid + iOS適用用
* **React Native**: React Nativeを使用したAndroid + iOS アプリケーションの場合

Adobe Experience Platform データ収集のタグ UI には、Web SDKおよび Mobile SDK拡張機能が含まれます。

上記の SDK でプロジェクトのニーズに対応できない場合は、[Adobe Experience Platform Edge Network API](https://developer.adobe.com/data-collection-apis/docs/) を使用して、データをAdobeに直接送信できます。

## データ収集プロセス

Adobe製品ごとに個別のライブラリをインストールして実装する代わりに、上記の SDK またはタグ拡張機能の 1 つを実装して、必要なすべてのデータを 1 つのペイロードに集計できます。 そのペイロードはAdobe Experience Platform Edge Networkの [&#x200B; データストリーム &#x200B;](/help/datastreams/overview.md) に送信されます。

![&#x200B; データ収集図 &#x200B;](assets/tags-sdks.png)

Adobe Experience Platform Edge Networkは、世界中に分散した信頼性の高い高速サーバーネットワークで、データを極めて大規模に受信および処理できます。 データストリームは、データを受信すると、設定した各ソリューションにそのデータを配信します。 データは、個々の製品が認識できる形式で渡されます。

![Adobe ソリューションの図 &#x200B;](assets/adobe-solutions.png)

また、[&#x200B; イベント転送 &#x200B;](/help/tags/ui/event-forwarding/overview.md) を使用して、クライアントサイドの実装コードを使用せずに、待ち時間の短い、Adobe以外の任意の宛先にデータを変換、エンリッチメントおよび送信することもできます。

![&#x200B; イベント転送図 &#x200B;](assets/event-forwarding.png)
