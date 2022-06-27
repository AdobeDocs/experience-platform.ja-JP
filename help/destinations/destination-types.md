---
keywords: 宛先；宛先；宛先のタイプ
title: 宛先のタイプとカテゴリ
description: Adobe Experience Platformの様々なタイプおよびカテゴリの宛先について説明します。
exl-id: 7826d1e2-bd6b-4f65-9da9-0a3b3e8bb93b
source-git-commit: a6fe0f5a0c4f87ac265bf13cb8bba98252f147e0
workflow-type: tm+mt
source-wordcount: '642'
ht-degree: 38%

---

# 宛先のタイプとカテゴリ

Adobe Experience Platformの宛先の様々なタイプとカテゴリについては、このページを参照してください。

## 宛先のタイプ {#destination-types}

Adobe Experience Platformでは、接続と拡張機能の 2 つのタイプの宛先を区別します。 接続の宛先には、プロファイルの書き出しの宛先と、セグメントの書き出しの宛先があります。

![宛先のタイプ](./assets/destination-types/types-of-destinations.png)

## 接続 {#connections}

**[!UICONTROL プロファイルの書き出し]**, **[!UICONTROL ストリーミングセグメントの書き出し]**、および **[!DNL Edge Personalization]** Adobe Experience Platformの宛先は、イベントデータをキャプチャし、他のデータソースと組み合わせて、を形成します。 [リアルタイム顧客プロファイル](../profile/home.md)、セグメント化の適用、セグメントおよび絞り込まれたプロファイルの書き出しをおこないます。

## プロファイルの書き出し先 {#profile-export}

プロファイルの書き出し先は生データを受け取り、多くの場合、電子メールアドレスをプライマリキーとして使用します。 Experience Platformは、現在、次の 2 種類のプロファイル書き出し先をサポートしています。

* [ストリーミングプロファイルの書き出し先（エンタープライズの宛先）](#streaming-profile-export)
* [バッチ（ファイルベース）の宛先](#file-based)

### ストリーミングプロファイルの書き出し先（エンタープライズの宛先） {#streaming-profile-export}

>[!IMPORTANT]
>
>エンタープライズの宛先（ストリーミングプロファイルの書き出し先）は、次の用途で使用できます。 [Real-time Customer Data Platform Ultimate](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform.html) のお客様のみ。

企業の宛先 Data Connectors を使用して、Real-time Customer Data Platformプロファイルをほぼリアルタイムで内部システムや他のサードパーティシステムに配信し、データ同期、分析、さらにプロファイルエンリッチメントの使用例をおこないます。

これらの宛先は、セグメントデータとプロファイルデータをExperience Platformデータストリームとして受け取ります。

企業の宛先には、次のものが含まれます。

* [HTTP API の宛先](catalog/streaming/http-destination.md)
* [Amazon Kinesis](catalog/cloud-storage/amazon-kinesis.md)
* [Azure Event Hubs](catalog/cloud-storage/azure-event-hubs.md)

### バッチ（ファイルベース）の宛先 {#file-based}

受け取るファイルベースの宛先 `.csv` プロファイルや属性を含むファイル。 [Amazon S3](catalog/cloud-storage/amazon-s3.md) は、プロファイルの書き出しを含むファイルを書き出せる宛先の例です。

## ストリーミングセグメントの書き出し先 {#streaming-destinations}

セグメントの書き出し先は、Experience Platformセグメントデータを受け取ります。 これらの宛先では、セグメント ID またはユーザー ID を使用します。 広告およびソーシャルの宛先（など） [[!DNL Google Display & Video 360]](catalog/advertising/google-dv360.md), [[!DNL Google Ads]](catalog/advertising/google-ads-destination.md)または [Facebook](catalog/social/facebook.md) は、このような宛先の例です。

## Edge パーソナライゼーションの宛先 {#edge-personalization-destinations}

Experience Platform内のエッジパーソナライゼーションの宛先には、以下が含まれます [Adobe Target](/help/destinations/catalog/personalization/adobe-target-connection.md) そして [カスタムパーソナライゼーションの宛先](/help/destinations/catalog/personalization/custom-personalization.md). これらの宛先を使用すると、顧客に対して同じページおよび次のページのパーソナライゼーションの使用例を有効にできます。

詳しくは、 [同じページと次のページのパーソナライゼーション用にパーソナライゼーションの宛先を設定](/help/destinations/ui/configure-personalization-destinations.md).

## プロファイルの書き出しとセグメントの書き出し先 — ビデオの概要 {#video}

次のビデオでは、2 種類の宛先の詳細について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/29707?quality=12)

## 拡張機能 {#extensions}

Platform は、タグ管理の機能と柔軟性を活用し、データ収集 UI でタグ拡張を設定できます。

>[!TIP]
>
>タグ拡張機能について詳しくは、使用例を含め、インターフェイスでのタグ拡張機能の見つけ方については、 [タグ拡張機能の概要](./catalog/launch-extensions/overview.md).

タグの拡張は、生のイベントデータを複数のタイプの宛先に転送します。 この拡張機能は、**イベント転送**&#x200B;タイプの宛先であると考えることができます。これは、宛先プラットフォームと単純に統合された機能で、イベントの生データを転送するだけです。例としては、[Gainsight パーソナライズ機能拡張機能](./catalog/personalization/gainsight.md)や [Confirmit Voice of the Customer 拡張機能](./catalog/voice/confirmit-digital-feedback.md)などがあります。

![他の宛先と比較したタグ拡張](./assets/common/launch-and-other-destinations.png)

## 接続と拡張機能を使用するタイミング {#when-to-use}

マーケターは、接続と拡張機能の組み合わせを使用して、お客様の使用例に対処できます。

接続は、完全な一元化された顧客プロファイルまたは顧客セグメントをアクティベーションに活用する必要がある場合に役立ちます。例えば、アップロードされた CRM データを含む Analytics システムの行動データを結合して、ユーザーにパーソナライズされたメッセージを配信する前に、特定のセグメントに対してそのユーザーを評価する場合、接続を使用します。

拡張機能は、イベントデータを使用してアクションをトリガーしたり、外部環境でセグメント化をおこなう場合に役立ちます。例えば、特定のユーザーについて、行動データをファイル上の他のデータソースと結合せずに、外部システムに転送する必要がある場合です。

## 宛先カテゴリ {#categories}

の接続と拡張機能 [宛先カタログ](https://platform.adobe.com/destination/catalog) は宛先カテゴリ (**広告**, **クラウドストレージ**, **調査プラットフォーム**, **電子メールマーケティング**&#x200B;など ) を、達成に役立つマーケティングアクションに応じて選択します。 各カテゴリについて、および各カテゴリに含まれる宛先について詳しくは、[宛先カタログのドキュメント](./catalog/overview.md)を参照してください。

![宛先カテゴリ](./assets/destination-types/destination-categories-menu.png)
